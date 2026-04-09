---
date: 2026-04-09T00:00:00Z
title: Why Your UniFi Captive Portal Isn't Redirecting (and How to Fix It)
seo:
  page_description: UniFi captive portal not popping up? This guide covers the top 7 causes — iOS CNA, DNS issues, HTTPS interception, firewall rules — and the exact fix for each.
  canonical_url:
  featured_image:
  author_twitter_handle:
  open_graph_type: article
  no_index: false
categories:
  - Captive Portal
  - Unifi Security
  - FAQ
author: Captivi Team
thumbImg:
  image_path: /images/blog/blog-thumb-3.jpg
featuredImg:
  image_path: /images/blog/featured-image-2.jpg
draft: false
---

Few things are more frustrating than deploying a captive portal, testing it on one device, and then discovering it silently fails on half your customers' phones. UniFi's external portal integration is solid when configured correctly — but there are at least seven distinct failure modes, and most of them produce the same symptom: the guest connects to the SSID and gets no internet access, with no portal appearing.

This guide covers each cause in detail with exact diagnostic steps and fixes. Bookmark it — if you manage UniFi captive portals, you'll need this.

## Why Captive Portal Redirects Fail: The Overview

The captive portal redirect mechanism is deceptively simple in theory:

1. Client connects to guest SSID
2. Client sends an HTTP request
3. UniFi intercepts it and returns a 302 redirect to the portal URL
4. Client loads the portal, authenticates
5. Portal calls UniFi API to authorize the client
6. Client gets internet access

Each of those five steps can fail independently, and the failure is almost always silent to the end user — they just get "no internet." The most common failure points are:

- **Detection probes:** iOS and Android use specific HTTP probe URLs to detect captive portals. If those probes aren't handled correctly, the native captive portal pop-up never appears.
- **DNS resolution:** Clients using encrypted or custom DNS may bypass your portal entirely.
- **HTTPS:** Most browser traffic is HTTPS today. The redirect only works on HTTP, which creates a class of failures where clients reach the internet without authenticating.
- **Walled garden / firewall rules:** If the portal server isn't whitelisted, clients can't load the portal page even when correctly redirected.
- **Controller URL configuration:** If the redirect points to an unreachable address, clients time out silently.

Let's go through each one.

---

## Cause 1 — iOS/Android Captive Network Assistant (CNA) Not Triggering

**What's happening:** iOS and Android don't wait for users to open a browser to detect captive portals. They make background HTTP probe requests when joining a new network:

- **iOS/macOS:** `http://captive.apple.com/hotspot-detect.html` — expects a 200 response with specific body text. If it gets anything else (including a redirect), it knows a captive portal is present and opens the CNA mini-browser.
- **Android:** `http://connectivitycheck.gstatic.com/generate_204` — expects a 204 No Content response. A redirect triggers the captive portal detection.

If the CNA pop-up never appears, the user has to open their browser manually — which may go to an HTTPS site, bypassing the redirect (see Cause 3).

**How to diagnose:**
From a device on your guest network, try opening `http://neverssl.com` in a browser. If the portal appears, the redirect is working — the CNA detection is the only issue. Check whether the probe URLs are in your walled garden.

**The fix:**
Add the following to your **Pre-Authorization Access (Walled Garden)** in UniFi:
- `captive.apple.com`
- `www.apple.com`
- `connectivitycheck.gstatic.com`
- `connectivitycheck.android.com`
- `msftncsi.com` (Windows Network Connectivity Status Indicator)
- `www.msftncsi.com`
- `ipv6.msftncsi.com`

These domains must be reachable from the guest network before authentication so the OS can make its probe requests. Once they're in the walled garden, the probe request goes through, hits your portal (which returns a redirect instead of the expected response), and triggers the CNA pop-up automatically.

---

## Cause 2 — Client is Using DNS-over-HTTPS (DoH) or a Custom DNS Resolver

**What's happening:** Captive portal redirect relies on DNS resolution going through the local DHCP-assigned DNS server — typically your router or a local resolver — which you can manipulate to ensure guests are pointed at your portal. But clients using encrypted DNS (DNS-over-HTTPS via Firefox, Cloudflare's `1.1.1.1` app, or Android's Private DNS feature) bypass your local DNS entirely.

More significantly: even without DoH, a client who has manually configured DNS to `8.8.8.8` or `1.1.1.1` receives valid DNS responses directly from Google or Cloudflare. If they then attempt to browse to a site, the DNS lookup succeeds without touching your network's resolver — which means your walled garden hostnames may not resolve correctly in the portal-blocked state.

**How to diagnose:**
On the guest VLAN, set up a firewall rule that logs dropped DNS traffic to external resolvers (destination port 53, destination not your local DNS IP). If you see significant traffic, clients are using external DNS.

**The fix:**
In **UniFi Network → Firewall Rules**, add a rule on the Guest network (LAN IN or LAN LOCAL, depending on your topology):

1. **Block or redirect external DNS:** Create a rule that blocks outbound UDP/TCP port 53 to any destination except your local resolver. This forces DNS through your resolver, where you can ensure captive portal domains resolve correctly.
   - Action: Drop (or Reject)
   - Protocol: TCP/UDP
   - Destination Port: 53
   - Destination: Anything except your local DNS IP

For DoH (which runs over HTTPS and can't be blocked by port alone), the practical fix is to block known DoH providers by IP. The major ones are:
- Cloudflare: `1.1.1.1`, `1.0.0.1`
- Google: `8.8.8.8`, `8.8.4.4`
- Quad9: `9.9.9.9`

Block outbound port 443 to these IPs from the guest VLAN. This is a blunt instrument, but it's effective for preventing DoH bypass in a captive portal environment.

---

## Cause 3 — HTTPS-Only Sites Block the Redirect

**What's happening:** The captive portal redirect only works for HTTP traffic. When a browser attempts `http://example.com`, UniFi intercepts it. When a browser attempts `https://example.com`, the TCP connection goes directly to the destination server, there's nothing to intercept at the HTTP layer, and the connection fails silently (because the actual internet is blocked, but the HTTP redirect mechanism doesn't apply).

Modern browsers have increasingly aggressive HTTP-to-HTTPS upgrading behavior. Chrome and Firefox remember which sites support HTTPS (HSTS preloading) and attempt HTTPS directly, skipping the HTTP redirect entirely.

**How to diagnose:**
If users report that the portal works when they manually type `http://` in the address bar but not when they just type a domain name or click a bookmark, this is the cause.

**The fix:**
This is primarily solved by fixing Cause 1 (CNA detection). When the OS-level captive portal detection works correctly, users see the portal pop-up before they attempt any browser navigation. The CNA pop-up uses its own HTTP probe mechanism, which is not subject to HSTS.

Additionally, ensure your walled garden includes `neverssl.com` — this is a well-known permanently-HTTP site that never upgrades to HTTPS, useful for manual testing and as a reliable trigger for captive portal detection in edge cases.

For some browsers, particularly on desktop, you may want to document the troubleshooting step for users: "If the Wi-Fi portal doesn't appear automatically, open a new browser tab and navigate to http://captive.captivi.info" (or whatever HTTP URL you've set up as a portal trigger landing page).

---

## Cause 4 — Portal Domain Not Whitelisted Pre-Auth

**What's happening:** When a client is redirected to your portal URL, they need to be able to load that URL. But at the moment of the redirect, they have no internet access — they're in the pre-authorization state where all traffic is blocked except your walled garden entries.

If `portal.yourdomain.com` isn't in the walled garden, the client gets redirected to a URL they literally cannot load. The browser shows a connection timeout or DNS failure, and the user has no idea what happened.

**How to diagnose:**
Connect a device to the guest SSID. Open a browser and navigate to `http://neverssl.com`. Observe whether you get a redirect (confirming the portal is configured) but then the portal page fails to load (confirming the walled garden is missing the portal domain).

**The fix:**
In **UniFi Network → WiFi → Guest Control → Pre-Authorization Access**, add:
- Your portal server's domain (e.g., `portal.yourdomain.com`)
- The IP address of your portal server (belt-and-suspenders)
- Any CDN domains your portal page loads from (Google Fonts, Bootstrap CDN, etc.)
- Your analytics or tracking scripts, if any

After saving, test again. The portal page should now load fully when redirected.

---

## Cause 5 — UniFi Controller URL Mismatch

**What's happening:** When UniFi redirects a client to the portal URL, it appends the controller's address as a parameter in the redirect URL. After authentication, your portal needs to call the UniFi API at that controller address. If the controller's **Override Inform Host** setting is misconfigured — pointing to an internal LAN IP that external clients (and your portal server) can't reach — the API call fails and no authorization is issued.

**How to diagnose:**
Inspect the redirect URL that appears in the browser address bar when you arrive at the portal page. Look at the parameters. Does the controller address in the URL resolve to a reachable endpoint? Try loading `https://<controller-address>` directly in a browser. If you get a connection refused or timeout, the address is wrong.

**The fix:**
In **UniFi Network → System → Advanced**:
- Set **Override inform host** to your controller's public-facing IP address or fully-qualified domain name
- Ensure this address is reachable from both client devices on the guest SSID (to load the redirect URL in their browser) and from your portal server (to make the API call)

If your controller is behind dynamic IP (common on residential and small business ISP connections), you need either:
1. A DDNS service (DuckDNS, Dynu, etc.) that keeps the hostname updated with your current IP
2. A port forward on your router to expose the controller API externally
3. A managed service like Captivi that handles this without requiring a static IP or DDNS

---

## Cause 6 — Guest Isolation Blocking Portal Traffic

**What's happening:** UniFi's **Client Device Isolation** setting (recommended for guest networks) prevents guest clients from communicating with other devices on the same network segment. This is correct behavior. However, some network topologies route guest traffic to the portal server through the local LAN, and if that route passes through the same segment where isolation is active, the portal traffic is blocked.

**How to diagnose:**
From a guest client, run: `curl -v http://portal.yourdomain.com`. If the connection hangs with no response — rather than being redirected — isolation may be blocking the path to the portal server.

**The fix:**
Ensure your portal server is reachable via an internet-routed path, not a local LAN route. Guest isolation blocks local-segment traffic; it does not block traffic that leaves the guest VLAN, traverses your router/gateway, and goes out to the internet (and potentially back in). If your portal server is on the same LAN as the guest VLAN gateway and you're using local routes, move the portal server to an internet-accessible host or change the routing configuration.

If you're using Captivi or another hosted portal service, this is not an issue — the portal is on the internet, and guest isolation doesn't affect internet-routed traffic.

---

## Cause 7 — Stale DHCP Lease or Client Cache

**What's happening:** This is less a configuration problem and more a testing artifact, but it causes enough confusion to be worth mentioning. A client device that has previously authenticated on your network may have a cached MAC authorization in the UniFi guest table — meaning it rejoins and immediately gets internet access without seeing the portal. Alternatively, a device with a stale DHCP lease from a previous session may behave unexpectedly until the lease renews.

**How to diagnose:**
In **UniFi Network → Clients → Guest Clients**, check whether your test device already appears as authorized from a previous session. If it does, revoke the authorization manually before testing.

**The fix:**
For testing, always:
1. Go to **UniFi Network → Clients** and find your test device
2. Select **Revoke Authorization** (for guest clients) or remove the MAC from authorized guests
3. Forget the Wi-Fi network on your test device before reconnecting
4. Use a fresh device if possible, or change MAC address on your test device

In production, this is less of an issue because session durations are typically short (hours, not days), and clients naturally re-authenticate when sessions expire.

---

## When DIY Troubleshooting Isn't Worth It

All seven of the issues above are solvable. But they require network administration knowledge, access to the UniFi controller, and time to diagnose and test. For a small business owner who just wants reliable guest Wi-Fi, none of this is a good use of time.

A managed captive portal service like Captivi handles all of these failure modes as part of the service. The walled garden is configured correctly out of the box. The CNA probe domains are included. The controller URL is managed. And when an edge case appears (and they do), the support team diagnoses it — not you.

If you're running guest Wi-Fi for a coffee shop, brewery, retail store, or similar hospitality business with UniFi gear and Square POS, this is exactly what Captivi is built for. See what's included on the [Features page](/feature/), or check [pricing](/pricing/) to find the tier that fits your operation.

---

## Need Help Getting Your Portal Working?

If you've worked through this guide and still can't get reliable redirects, [contact the Captivi team](/contact/). We've seen every variant of this problem across hundreds of UniFi deployments, and we're happy to help diagnose even if you're not a current customer.
