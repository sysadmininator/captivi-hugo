---
date: 2026-04-09T00:00:00Z
title: Best UniFi Captive Portal for Coffee Shops with Square POS (2026)
seo:
  page_description: Running a coffee shop with Square POS? See how Captivi's purchase-gated Wi-Fi stops bandwidth abuse and keeps your network secure — no data harvesting, ever.
  canonical_url:
  featured_image:
  author_twitter_handle:
  open_graph_type: article
  no_index: false
categories:
  - Square POS
  - Captive Portal
  - WiFi
author: Captivi Team
thumbImg:
  image_path: /images/blog/blog-thumb-3.jpg
featuredImg:
  image_path: /images/blog/featured-image-5.jpg
draft: false
---

Walk into any independent coffee shop on a Monday morning and you'll find the same scene: paying customers waiting for a seat while a row of laptops occupies every table, their owners nursing a single drip coffee for three hours. The Wi-Fi is the product — and it's being given away for free to anyone within range of your access points, regardless of whether they've spent a dollar.

If you're running a café on UniFi networking gear and Square POS, you have everything you need to fix this problem — you just need the right captive portal layer connecting them.

## The Coffee Shop Wi-Fi Problem

Free, open guest Wi-Fi made sense a decade ago when it was a differentiator. Today it's table stakes, and it creates real operational problems:

- **Bandwidth abuse:** A single person streaming video or on a video call can consume more bandwidth than twenty regular browsers. Slow Wi-Fi is one of the most common Google review complaints for cafés.
- **Seat squatting:** "Laptop campers" who don't purchase anything displace paying customers, reduce table turnover, and hurt revenue — especially during peak hours.
- **Security exposure:** An open guest network with no segmentation puts your payment terminals and back-office systems at risk.
- **No insight:** You have zero visibility into who is actually using your network or when congestion peaks occur.

The traditional response — posting a Wi-Fi password on the chalkboard — addresses none of these issues. Anyone who asks gets the password, and once they have it, there's no mechanism to re-verify that they're an active customer.

## Why Receipt-Verified Guest Access is the Solution

The most effective solution for coffee shops is **purchase-gated Wi-Fi**: guests receive Wi-Fi access only after completing a transaction. This ties network access directly to the thing that actually keeps your business running — a sale.

Here's why it works:

- **Only paying customers connect.** The Wi-Fi becomes a benefit of purchasing, not a public utility.
- **Natural session management.** A receipt code is tied to a transaction, which means access can be time-limited from the moment of purchase — mirroring how long a reasonable visit should last.
- **No sign-up friction for the customer.** They enter a code from their receipt. No email required. No account to create. Ten seconds and they're online.
- **You stay out of the data business.** No email lists, no marketing opt-ins — just clean, functional guest access.

## How Captivi Works with Square POS

Captivi is purpose-built for exactly this use case. It sits between your UniFi controller and your Square account, validating receipt codes in real time.

Here's the end-to-end flow:

1. **Customer completes a purchase** at your Square POS terminal (counter, tablet, or Square Register).
2. **Customer opens their laptop or phone** and connects to your guest Wi-Fi SSID.
3. **The captive portal appears automatically** — a branded page with your logo and colors.
4. **Customer enters the order ID** from their Square receipt (printed or digital).
5. **Captivi verifies the code** against the Square API in real time.
6. **Access is granted** for a configurable session window (e.g., 2 hours, 4 hours).
7. **After the session expires**, the customer sees the portal again — an implicit nudge to order something else if they want to stay online.

The entire handshake takes under 10 seconds. No staff involvement required.

For the technical side: Captivi connects to your UniFi controller via the UniFi External Guest Portal API. When a code is validated, Captivi signals the controller to authorize the client MAC address. Your UniFi equipment handles all the actual Wi-Fi delivery — Captivi is purely the authentication layer.

## Branded Portal Experience

First impressions matter, and your captive portal is part of the customer experience. Captivi's portal shows your branding — your café's logo, your color scheme, your Wi-Fi network name — not ours.

This matters for a few reasons:

- **Professionalism.** A polished portal signals that your business is intentional about the guest experience. A generic "Enter password" screen doesn't.
- **Brand consistency.** Customers associate the smooth Wi-Fi experience with your brand, not a third-party tool they've never heard of.
- **Trust.** A portal that looks like it belongs to your café is more trustworthy than an unknown redirect page — especially for customers who are security-conscious.

Portal setup is handled during onboarding. You provide your logo and brand colors, and the Captivi team configures the portal to match. No design work required on your end.

## Privacy-First: What Captivi Does NOT Do

This is worth stating plainly, because many guest Wi-Fi solutions monetize the data they collect from your customers.

**Captivi does not:**
- Collect email addresses from your guests
- Display third-party advertising
- Build marketing profiles on your customers
- Sell or share guest data with anyone
- Require customers to create an account

Receipt code verification is a zero-knowledge process from the customer's perspective. We validate the code, grant access, and move on. Your customers' browsing habits, device identifiers, and personal information are none of our business — and none of yours, either, unless they choose to share it directly with you.

For coffee shop owners who care about customer trust and increasingly savvy guests who know what data collection looks like, this is a meaningful differentiator.

## Setup Requirements

To use Captivi in your coffee shop, you need:

- **A UniFi controller** — self-hosted (UniFi Network Application) or cloud-hosted (UniFi Site Manager). Works with UniFi Dream Router, Dream Machine, Cloud Gateway, and legacy Security Gateways.
- **At least one UniFi access point** configured as a guest SSID. Any current-generation UniFi AP (U6 Lite, U6 Pro, U6 Long-Range, etc.) works perfectly.
- **A Square POS account** with active transactions. Any Square plan works, including the free tier.
- **A network connection from your controller to the internet.** Captivi communicates with your controller via the cloud. If you're on a standard ISP connection, CloudRoute (available on the Managed tier) eliminates the need for a static IP or DDNS setup entirely.

There is no additional hardware required. No new router, no dedicated server, no on-site appliance. Your existing equipment does the heavy lifting.

## Pricing for Coffee Shops

Captivi offers three monthly plans, all available for a single location:

- **Secured ($50/month):** Core captive portal with Square receipt verification, branded portal, and email support. Ideal for a café that wants purchase-gated Wi-Fi and a set-it-and-forget-it solution.
- **Monitored ($100/month):** Adds UniFi network monitoring and chat support. If you want visibility into network health without hiring an IT person, this is the right tier.
- **Managed ($250/month):** Full-service — proactive network recommendations, phone support, and CloudRoute (no static IP required). Best for multi-location operators or cafés with complex network setups.

All plans include a one-time setup fee of $250, which covers branded portal configuration, Square integration, and UniFi setup. The setup fee is waived for Managed tier customers.

See the full breakdown on the [Captivi Pricing page](/pricing/).

## Frequently Asked Questions

**Do customers need an account to get Wi-Fi?**
No. They enter the order ID from their Square receipt. Nothing else required.

**What if a customer loses their receipt or orders for cash?**
This is a real edge case for some shops. If you run a high volume of cash sales, Captivi's session-based access means a staff member can manually grant a short session through the UniFi portal. For card-heavy cafés (which is most of them), this is rarely an issue.

**Does it work with digital receipts?**
Yes. Whether the receipt is printed, emailed, or shown in the Square app on the customer's phone, the order ID is the same.

**What happens when the session expires?**
The customer is redirected back to the captive portal. They can either enter a new receipt code from a subsequent purchase or simply stop using the Wi-Fi. There is no disruption to anyone else on the network.

**Does it slow down the Wi-Fi?**
No. Captivi is purely an authentication layer. Once a client is authorized, all traffic flows directly through your UniFi access points. There is no traffic proxying or throughput impact.

**Can I set different time limits for different purchases?**
Session duration is configurable at the account level. If you want to set it to 2 hours for a standard coffee order, that's a simple configuration choice during setup.

---

## Get Started

If you're running a coffee shop on UniFi gear with Square POS, Captivi is the fastest path to purchase-verified guest Wi-Fi that works without lifting a finger after setup.

[Contact us to get started](/contact/) — we'll walk you through requirements for your specific setup and get you up and running quickly.

Want to see everything Captivi can do? [Explore the full feature list](/feature/).
