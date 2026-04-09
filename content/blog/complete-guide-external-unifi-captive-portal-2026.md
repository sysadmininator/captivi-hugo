---
date: 2026-04-09T00:00:00Z
title: The Complete Guide to Setting Up an External UniFi Captive Portal in 2026
seo:
  page_description: Step-by-step guide to deploying an external captive portal on UniFi in 2026. Covers guest SSID setup, External Portal settings, firewall rules, and real-world testing.
  canonical_url:
  featured_image:
  author_twitter_handle:
  open_graph_type: article
  no_index: false
categories:
  - Captive Portal
  - Unifi Security
  - WiFi
author: Captivi Team
thumbImg:
  image_path: /images/blog/blog-thumb-6.jpg
featuredImg:
  image_path: /images/blog/featured-image-2.jpg
draft: false
---

UniFi's built-in Hotspot Manager handles basic guest Wi-Fi well, but it has a ceiling. No custom authentication logic, no integration with external databases or POS systems, no programmatic session management. When you need guest access that does more than distribute voucher codes, you need an external captive portal.

This guide walks through every step of configuring an external captive portal on UniFi in 2026 — from initial SSID setup through firewall rules, authentication flow, and real-world testing. It's written for network administrators who want to understand what's actually happening under the hood, not just copy-paste a configuration.

## What is an External Captive Portal?

A captive portal is a web page that intercepts a connecting client's traffic and redirects them to an authentication page before granting internet access. You've encountered one every time you connected to hotel Wi-Fi or an airport lounge.

UniFi supports two captive portal modes:

1. **Internal (Hotspot Manager):** The portal is served directly by the UniFi controller. Authentication is limited to voucher codes, a simple password, or no authentication at all. Configuration is done entirely within the UniFi web interface.

2. **External:** The portal is served by a third-party web server. UniFi handles the initial redirect, but your external server handles authentication, session management, and the user experience. When authentication is complete, the external portal calls the UniFi API to authorize the client's MAC address.

External portals are more complex to set up, but they unlock capabilities that the built-in manager simply cannot provide: POS integration, custom authentication flows, branded experiences, RADIUS-based accounting, and more.

## Prerequisites

Before starting, confirm the following:

- **UniFi Network Application (UNA) 8.x or later**, or UniFi OS 3.x on a Dream Machine/Cloud Gateway. The external portal API has been stable since firmware 2.x, but current versions have the cleanest implementation.
- **A dedicated guest SSID.** Don't try to add captive portal authentication to an existing production SSID. Create a new one.
- **A static IP or DDNS hostname for your UniFi controller.** Your external portal server needs to call the UniFi API, which means it needs a stable address for your controller. If you're using a managed service like Captivi, CloudRoute eliminates this requirement.
- **An external portal server accessible via HTTPS.** Self-signed certificates cause problems with modern operating systems (iOS in particular). Use a valid certificate — Let's Encrypt is free and works well.
- **Network access to UniFi controller API (port 443, or 8443 on older installs).** The external portal's server needs to be able to reach your controller's API endpoint.

## Step 1 — Create a Guest SSID with a Dedicated VLAN

Navigate to **UniFi Network → WiFi → Create New WiFi Network**.

Configuration settings:
- **Name:** Your guest network name (e.g., "YourBusiness-Guest")
- **Password:** Leave empty for open authentication — the captive portal handles authorization. Or set a simple shared password if you want one layer of basic access control before the portal.
- **Network (VLAN):** Create a new guest network here, or select an existing guest VLAN. **Do not use your main LAN.** A dedicated VLAN ensures:
  - Guest clients cannot reach your management devices
  - Firewall rules can be applied specifically to this segment
  - Bandwidth limiting applies cleanly

Under **Advanced:**
- Enable **Client Device Isolation** — prevents guests from scanning or communicating with each other on the same network.
- Set **Rate Limiting** if desired (e.g., 20 Mbps down / 10 Mbps up per client). This prevents individual clients from saturating your connection.

Save the SSID. Clients can now connect but will receive no internet access until authentication is complete — a fact we'll use in Step 4.

## Step 2 — Enable External Captive Portal in UniFi

This is the core configuration step. Navigate to:

**UniFi Network → WiFi → [Your Guest SSID] → Edit → Advanced**

Scroll to the **Guest Portal** section:
- Set **Portal** to **External Portal**
- **External Portal URL:** Enter the full HTTPS URL of your portal page (e.g., `https://portal.yourdomain.com/`)
- **Redirect using hostname:** Toggle this on if you want the redirect URL to use your controller's hostname rather than its IP. Recommended for cleaner URLs in the browser.

Under **Controller settings** (found in System → Advanced):
- Ensure **Override inform host** is set to your controller's public hostname or IP. This is the address the UniFi controller will use in the redirect URL it sends to connecting clients. If this is wrong, clients will get a redirect to an internal IP that their devices cannot reach.

Save these settings. At this point, connecting clients will be redirected to your portal URL when they try to browse the web.

### How the Redirect Works

When a client connects to the guest SSID and attempts an HTTP request, UniFi's guest control intercepts it and responds with an HTTP 302 redirect to your portal URL, appending the following query parameters:

```
https://portal.yourdomain.com/?
  id=<client-mac-address>
  &ap=<access-point-mac>
  &ssid=<ssid-name>
  &t=<timestamp>
  &url=<original-destination-url>
  &site=<site-id>
```

Your portal uses `id` (the client MAC address) to identify the client that needs authorization. After successful authentication, your portal calls the UniFi API to authorize that MAC address.

**Note:** HTTPS sites won't trigger this redirect, which is why iOS and Android use special HTTP probe URLs to detect captive portals (more on this in the troubleshooting article). Always test your portal redirect using an HTTP destination first.

## Step 3 — Configure the Portal Server

This step depends entirely on your portal software. If you're building a custom portal, you need to implement:

1. **A landing page** that displays the authentication form
2. **Authentication logic** that validates credentials against your data source (voucher database, POS system, RADIUS server, etc.)
3. **UniFi API call** to authorize the client on successful authentication

The UniFi API call to authorize a guest is a POST request to:

```
POST https://<controller-address>/api/s/<site>/cmd/stamgr
Content-Type: application/json

{
  "cmd": "authorize-guest",
  "mac": "<client-mac-address>",
  "minutes": 120
}
```

You need an active UniFi admin session (cookie-based auth) or an API key (available in newer UniFi OS versions) to make this call. The `minutes` parameter sets session duration.

After a successful POST, the client's MAC address is added to the authorized list and internet access is granted. The client's browser is redirected to the `url` parameter that was passed in the original redirect query string (their original destination).

If you're using a managed service like Captivi, all of this is handled for you — you point UniFi at the Captivi portal URL, and Captivi manages the API integration.

## Step 4 — Whitelist the Portal Domain in Pre-Auth Firewall Rules

Here's the catch: when a client first connects, they have no internet access. They can't load your portal page if the portal is on the internet and all internet traffic is blocked.

UniFi solves this through **Pre-Authorization Access** (also called "walled garden"). You whitelist specific domains that clients can reach before authenticating.

Navigate to **UniFi Network → Profiles → Pre-Authorization Access** (or find it under Guest Control in older UNA versions).

Add your portal domain:
- `portal.yourdomain.com` (your portal server)
- Any CDN domains your portal page loads assets from (fonts, stylesheets, images)
- Any API domains your portal calls from the client-side

Also add the following for platform functionality:
- Your UniFi controller domain (if using hostname-based redirects)
- Any IP ranges for your portal server

**Testing this:** Connect a device to the guest SSID before adding any entries. Open `http://detectportal.firefox.com/success.txt` (this is one of Firefox's captive portal detection URLs and will trigger a redirect). Confirm you reach your portal page. Then try loading a resource that your portal depends on. If it fails, that domain or IP needs to be added to the walled garden.

## Step 5 — Test the Authentication Flow

Testing is where most deployments surface problems. Follow this sequence:

### Test 1: Redirect Trigger

Connect a device to the guest SSID. Open a browser and navigate to `http://neverssl.com` (a permanently HTTP-only site maintained specifically for this use case). You should be redirected to your portal page. If you're not:
- Check that the guest SSID has the External Portal setting applied and saved
- Verify the portal URL is correct and reachable from the internet
- Check for typos in the controller hostname override

### Test 2: Pre-Auth Access

Confirm your portal page loads fully — all CSS, JavaScript, and images. Any broken assets likely indicate a missing walled garden entry.

### Test 3: Authentication and Authorization

Complete the authentication flow on your portal (enter a voucher code, receipt code, credentials, etc.). After submission, your portal server should call the UniFi API to authorize the client. Check:
- The portal server can reach your controller's API endpoint (test with `curl` from the server)
- The API credentials or API key are valid
- The MAC address is being passed correctly from the redirect query parameters

After successful authorization, navigate to **UniFi Network → Clients → Guest Clients** and confirm your test device appears as authorized.

### Test 4: Session Expiry

Wait for the session to expire (or shorten the duration for testing) and confirm the client is re-prompted by the portal. If they can continue browsing after expiry, check that the session duration parameter in the API call is being set correctly.

### Test 5: iOS and Android Captive Network Assistant

Connect with an iPhone. The Captive Network Assistant (CNA) — Apple's built-in pop-up browser that appears when joining captive portal networks — should trigger automatically. If it doesn't:
- Ensure your walled garden includes Apple's captive portal detection domains: `captive.apple.com`, `www.apple.com`
- Check that HTTP 302 redirects are working for HTTP traffic (CNA uses HTTP probes)

Connect with an Android device and verify similar behavior. Android uses `connectivitycheck.gstatic.com` as its detection probe.

## Common Pitfalls and How to Avoid Them

**HTTPS redirect failure:** Modern browsers assume popular sites are HTTPS and will attempt HTTPS before HTTP. Since your portal redirect only works on HTTP, the CNA probe and HTTP-first test work correctly, but manual attempts to visit `https://google.com` won't trigger the redirect. This is expected behavior. The redirect is triggered by the captive portal detection mechanism, not by the first user browse.

**Controller hostname vs. IP mismatch:** If your controller is behind NAT (typical home/SMB setup), the "Override inform host" must be set to the public-facing IP or hostname, not the LAN IP. Clients are on the internet side — they can't reach `192.168.1.1`.

**DNS resolving to internal IP:** If your controller's hostname resolves to an internal IP from the outside, your portal can't call the API. Use a split-horizon DNS configuration or expose the API on a different public endpoint.

**API authentication failing:** UniFi OS requires either cookie-based auth (login with credentials before each session) or API keys (preferred in newer versions). If you're using an older admin account with 2FA enabled, API key access is cleaner.

**Client isolation blocking portal traffic:** If **Client Device Isolation** is on (as recommended), clients on the same segment can't see each other. This is fine for client-to-client traffic, but ensure your portal server is reachable via the internet path, not a LAN route that would be blocked by isolation.

## DIY vs. Managed Service: When Captivi Makes More Sense

Building and maintaining an external captive portal is meaningful engineering work. You need to run a web server, manage TLS certificates, maintain UniFi API compatibility across firmware updates, handle session state, and deal with the edge cases that accumulate in production (iOS CNA quirks, DNS over HTTPS bypasses, stale MAC authorizations).

If your goal is a custom authentication flow for a complex enterprise deployment — RADIUS integration, per-SSID policy enforcement, detailed analytics — the DIY approach gives you maximum flexibility.

If your goal is purchase-verified guest Wi-Fi for a small business (coffee shop, brewery, retail store), building this yourself is overkill. The configuration described above, combined with the Square POS integration, would require a web server, backend code, and ongoing maintenance that most small business operators don't have time for.

Captivi is a managed service that handles all of this. You point your UniFi controller at Captivi's portal URL, provide your Square credentials during onboarding, and the integration runs autonomously. No server to maintain, no API compatibility to manage, no certificate renewals to forget. See everything Captivi handles on the [Features page](/feature/).

## Pricing and Getting Started

If you're building a custom portal, the main costs are server hosting and your own development time.

If you're deploying for a business and want a production-ready solution without the engineering overhead, [Captivi's pricing](/pricing/) starts at $50/month for a single location, with a one-time $250 setup fee.

---

## Summary

Setting up an external UniFi captive portal in 2026 requires:

1. A dedicated guest SSID with VLAN isolation
2. External Portal enabled in UniFi with your portal URL
3. Correct controller hostname override for the redirect URL
4. Pre-authorization access (walled garden) for your portal domain
5. A portal server that calls the UniFi API to authorize clients
6. Testing across multiple devices and browsers

It's a well-documented capability of the UniFi platform, and when configured correctly, it's robust and reliable. The complexity is in the details — and particularly in the testing.

[Contact us](/contact/) if you're looking for a managed solution that handles this configuration for your business.
