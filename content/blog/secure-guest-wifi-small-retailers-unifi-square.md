---
date: 2026-04-09T00:00:00Z
title: "Secure Guest Wi-Fi for Small Retailers: UniFi & Square Setup Guide"
seo:
  page_description: Learn how small retailers using UniFi and Square POS can deploy secure, purchase-verified guest Wi-Fi — without IT expertise or expensive hardware upgrades.
  canonical_url:
  featured_image:
  author_twitter_handle:
  open_graph_type: article
  no_index: false
categories:
  - Square POS
  - Unifi Security
  - WiFi
author: Captivi Team
thumbImg:
  image_path: /images/blog/blog-thumb-3.jpg
featuredImg:
  image_path: /images/blog/featured-image-5.jpg
draft: false
---

Small retail stores face a Wi-Fi dilemma that's easy to underestimate. On one hand, customers increasingly expect to connect while they browse — especially in boutiques, specialty stores, and shops where customers spend meaningful time. On the other hand, offering open guest Wi-Fi introduces real security risks, operational headaches, and no mechanism to ensure the network is being used by people who are actually there to shop.

If your store runs on UniFi networking gear and Square POS, you already have the building blocks for a solution that addresses all of this — secure, segmented, purchase-verified guest access that requires no IT expertise to maintain.

## The Retail Wi-Fi Security Risk

The most pressing concern for any retailer who accepts card payments is PCI-DSS compliance. The Payment Card Industry Data Security Standard has explicit requirements around network segmentation: cardholder data environments must be isolated from other network traffic. Running Square terminals on the same network segment as open guest Wi-Fi is a compliance red flag.

Even without formal audits, the practical risk is real. An open or weakly secured guest network is a vector for:

- **Network scanning and reconnaissance** by bad actors who can see other devices on the same segment
- **Man-in-the-middle attacks** targeting customers on the same network
- **Bandwidth consumption** by customers (or passers-by) that degrades the quality of your payment processing connection
- **Unauthorized access** to network-connected devices like printers, cameras, or back-office computers

The good news is that UniFi's architecture already handles most of this through VLANs. The issue is that a lot of small retailers have UniFi gear configured with everything on a flat network — it came that way from the installer, or it was set up years ago before security concerns were front of mind.

## VLAN Segmentation: Staff vs. Guest Traffic

A properly configured UniFi setup separates network traffic into distinct VLANs (Virtual Local Area Networks). The key separation for a retail environment is:

- **Management/Staff VLAN:** Square terminals, back-office computers, printers, security cameras. This VLAN has direct internet access and is isolated from all other traffic.
- **Guest VLAN:** Customer Wi-Fi. This VLAN can reach the internet but cannot reach the management VLAN or any devices on it. Even if a guest device is compromised, it cannot see your Square terminal.

UniFi makes this configuration straightforward at the controller level. Network → Create New Network → set as "Guest Network" automatically applies client isolation and blocks inter-VLAN routing. An external captive portal integration like Captivi layers on top of this native UniFi capability without requiring any additional configuration.

When Captivi is deployed on the guest SSID, the authentication layer sits at the edge of the guest VLAN. Customers authenticate against the portal before being granted internet access — but at no point do they ever have visibility into your management VLAN. The separation is architectural, not just policy-based.

## Purchase-Verification: Stop Unauthorized Use

An open guest network in a busy retail strip isn't just being used by your customers. It's being used by people in adjacent shops, people on the sidewalk, and anyone who ever got the password and kept it saved on their device.

Purchase verification changes this. With Captivi, guests enter the order ID from their Square receipt to access Wi-Fi. No receipt, no access. This simple mechanic means:

- **Pedestrians can't free-ride your connection.** They can see the SSID, but the portal blocks access until a code is verified.
- **Old connections don't persist.** Session-based access means a code from last Tuesday doesn't still work today — each visit requires a current purchase.
- **Bandwidth is reserved for customers.** Peak retail hours stop being congested with background traffic from non-customers.

For a boutique or specialty retailer where customer experience matters, this is also just polished. "Our Wi-Fi is available to customers — enter the code from your receipt" is a much cleaner story than "the password is on the counter, but please don't abuse it."

## Privacy-First by Design

Many guest Wi-Fi platforms are built around data collection. You install them, you offer free Wi-Fi, and in the background, the platform is building profiles on your customers — email addresses, device identifiers, dwell time, repeat visit patterns. Some of this data is sold to third-party marketers. Some is used to run advertising targeting against your own customers.

This is increasingly problematic from a compliance standpoint (GDPR, CCPA, and similar regulations all have implications for this kind of passive data collection) and from a trust standpoint. Customers are more privacy-aware than they were five years ago.

Captivi collects what's needed to authenticate and manage sessions, and nothing else. There is no email capture, no marketing profile building, no device fingerprinting for advertising purposes. Your customers' data stays with you and with Square, where it's always been.

For small retailers who stake their reputation on community trust, this matters. You don't want to be the shop whose "free Wi-Fi" was found to be harvesting customer data. With Captivi, you're not.

## No Extra Hardware Required

The concern we hear most often from small retailers considering a captive portal upgrade is the assumption that it requires new hardware — a dedicated server, an additional router, or specialized networking equipment.

Captivi runs entirely in the cloud. The only hardware you need is what you already have:

- **A UniFi controller** (Dream Router, Dream Machine Pro, Cloud Gateway Ultra, or a self-hosted UniFi Network Application on a small server or Raspberry Pi)
- **UniFi access points** — any current-generation APs work, from the affordable U6 Lite to the high-density U6 Pro
- **A Square POS account** with your existing terminals

The Captivi service connects to your UniFi controller and your Square account over standard HTTPS. There's nothing to rack-mount, no additional subscriptions for third-party hardware, and no ongoing maintenance of a local appliance.

## Setup Simplicity: What Captivi Handles for You

One of the persistent barriers to small retail operators upgrading their guest Wi-Fi is the assumption that it requires technical expertise. Captivi's onboarding is specifically designed to remove this barrier.

When you sign up, the Captivi team handles:

- **Portal design:** Your logo, colors, and branding are applied to the captive portal page.
- **Square integration:** The connection between the Captivi platform and your Square account is configured and tested.
- **UniFi configuration:** The External Guest Portal settings in your UniFi controller are configured to point to Captivi's portal URL. Firewall rules for pre-authentication access are set up correctly.
- **Testing:** End-to-end testing confirms that receipt codes validate correctly and sessions work as expected.

Your job during onboarding is to provide your logo, your brand colors, your Square API credentials, and access to your UniFi controller. The Captivi team takes it from there.

After setup, the system runs autonomously. There's no daily management required. If something goes wrong, support is available via email (Secured tier), chat (Monitored and Managed tiers), or phone (Managed tier).

See the complete feature list on the [Captivi Features page](/feature/).

## Pricing for Retail

Captivi's pricing is designed for small business budgets:

- **Secured ($50/month):** The essentials — captive portal with receipt verification, branded design, and email support. This is the right choice for most single-location retailers.
- **Monitored ($100/month):** Adds UniFi network monitoring and chat support. If you want someone watching your network health proactively — especially valuable during high-traffic seasons like the holidays — this tier is worth considering.
- **Managed ($250/month):** Comprehensive management including proactive recommendations, phone support, and CloudRoute (no static IP or DDNS required). Suited for multi-location retailers or stores with complex network environments.

All plans include a one-time setup fee of $250 covering portal configuration, Square integration, and network setup. This fee is waived for Managed tier customers.

Full pricing details on the [Captivi Pricing page](/pricing/).

## Frequently Asked Questions

**Does it work with the UniFi Dream Machine?**
Yes. Captivi supports all UniFi controller platforms — UniFi Dream Machine, Dream Machine Pro, Dream Router, Cloud Gateway series, and self-hosted UniFi Network Application instances.

**What about multi-location retailers?**
Multi-site support is available on the Monitored tier (up to 3 sites) and Managed tier (up to 5 sites). If you have more than 5 locations, contact us to discuss a custom arrangement.

**Does Captivi replace my current UniFi setup?**
No. Captivi is an authentication layer that works alongside your existing UniFi configuration. You keep your current hardware, your current SSIDs, and your current network segmentation. Captivi adds the portal and the Square verification step on top.

**What if a customer doesn't have their receipt?**
Order codes appear on both printed and digital receipts. If a customer genuinely can't access their code (e.g., cash payment with no receipt printed), staff can manually grant a short session through the UniFi controller interface. For most card-heavy retail environments, this edge case is rare.

**Is there a contract?**
Captivi is a month-to-month subscription. There's no long-term contract or cancellation penalty.

---

## Get Secure Guest Wi-Fi in Your Store

If you're running a retail store on UniFi gear with Square POS, Captivi is the fastest path to genuinely secure, purchase-verified guest Wi-Fi — without a new hardware purchase, without an IT consultant, and without compromising your customers' privacy.

[Contact us to get started](/contact/). The Captivi team handles setup, and you'll be up and running with minimal disruption to your day-to-day operations.
