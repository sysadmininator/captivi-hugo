---
date: 2026-04-09T00:00:00Z
title: "UniFi Hotspot vs. External Captive Portal: Which is Better for Your Business?"
seo:
  page_description: Compare UniFi's built-in Hotspot Manager to an external captive portal. See which is right for your business size, technical skill level, and guest Wi-Fi goals.
  canonical_url:
  featured_image:
  author_twitter_handle:
  open_graph_type: article
  no_index: false
categories:
  - Captive Portal
  - WiFi
  - Unifi Security
author: Captivi Team
thumbImg:
  image_path: /images/blog/blog-thumb-6.jpg
featuredImg:
  image_path: /images/blog/featured-image-5.jpg
draft: false
---

If you're running a business on UniFi networking gear and looking at guest Wi-Fi options, you've probably encountered two distinct approaches: the built-in Hotspot Manager that comes with your UniFi controller, and the option to connect an external captive portal service. Both redirect guests through an authentication page before granting internet access. Beyond that, they're quite different products.

This guide breaks down exactly what each approach offers, where each falls short, and which one is right for your specific situation — based on your business type, technical comfort level, and what you actually need guest Wi-Fi to do.

## What is the UniFi Hotspot Manager?

The UniFi Hotspot Manager is a built-in feature of the UniFi Network Application (UNA). It's available at no additional cost as part of your UniFi controller software — no third-party subscription required.

It provides:

- **Voucher-based access:** Guests enter a single-use or multi-use voucher code to get online. You generate vouchers in bulk from the controller interface.
- **Password access:** A shared password for all guests on the network. Simple, but no per-session accountability.
- **Time and data limits:** Configure how long each voucher grants access (1 hour, 8 hours, 24 hours, etc.) and optional data limits per session.
- **Basic customization:** A portal page with your logo and limited color customization.
- **Usage logs:** The controller records connection times and basic usage data per voucher.

The Hotspot Manager is a self-contained system. Everything runs on your controller. There are no external dependencies, no API calls to third-party services, and no ongoing subscription fees.

### What the Hotspot Manager Does Not Do

- **POS integration:** There's no connection to Square, Clover, Toast, or any other point-of-sale system. Voucher codes are manually generated and distributed — typically printed on receipts, written on chalkboards, or handed to customers by staff.
- **Automated purchase verification:** There's no way to automatically tie Wi-Fi access to a purchase without significant custom development.
- **Branded portal with your exact design:** Customization is limited — you can add a logo, but deep brand matching isn't available.
- **Network monitoring:** The Hotspot Manager is purely an access control layer. It doesn't monitor network health.
- **Proactive support:** It's a self-managed tool. When something goes wrong, you debug it.

## What is an External Captive Portal?

An external captive portal is a third-party service that handles the authentication page and session management for your guest network. UniFi redirects connecting clients to the external portal URL, the portal handles authentication, and on success it calls the UniFi API to authorize the client's MAC address.

Because the portal logic lives outside the UniFi controller, external portals can implement any authentication mechanism imaginable: receipt code validation, RADIUS authentication, social login, identity verification, custom business logic.

The trade-off is complexity. An external portal requires:
- A portal server accessible via HTTPS
- API credentials for your UniFi controller
- Correct pre-authorization (walled garden) configuration
- Ongoing maintenance as UniFi firmware updates

For most small businesses, building and maintaining this infrastructure is not practical. That's why managed external portal services exist — they handle the technical complexity, and you interact with a simple dashboard.

## Feature Comparison

| Feature | UniFi Hotspot Manager | External Captive Portal |
|---|---|---|
| **POS integration (e.g., Square)** | No | Yes (with the right service) |
| **Automated purchase verification** | No | Yes |
| **Voucher code distribution** | Manual | N/A (different auth model) |
| **Branded portal** | Basic logo only | Full brand matching |
| **Custom authentication logic** | No | Yes |
| **Session duration control** | Yes | Yes |
| **Per-client bandwidth limits** | Yes | Yes (via UniFi) |
| **Usage reporting** | Basic | Varies by service |
| **Network health monitoring** | No | Available with some services |
| **Proactive support** | None | Available with managed services |
| **Setup complexity** | Low (built-in) | Medium to high |
| **Ongoing maintenance** | Self-managed | Varies (DIY or managed) |
| **Monthly cost** | $0 | Varies ($0 DIY – $250+/mo managed) |
| **Data privacy model** | Self-contained | Varies by provider |

## When the Built-in Hotspot Manager is Enough

The UniFi Hotspot Manager is a genuinely good tool for specific use cases. You should stick with it if:

**You're technically comfortable and don't need POS integration.** If you're a network admin managing a hotel or co-working space, manually generating voucher batches and distributing them through a paper or digital workflow is a reasonable operational model. The Hotspot Manager gives you the controls you need without a third-party dependency.

**You want zero external dependencies.** If connectivity to a third-party portal service goes down, your guests can't authenticate. The Hotspot Manager runs locally on your controller — there's no external service to fail.

**Your guest Wi-Fi requirements are simple.** If you just need to prevent unauthorized access with a basic password or voucher system and don't need purchase verification or branded design, the built-in manager does the job.

**You're running a test environment or home lab.** There's no reason to pay for an external service for a deployment that doesn't serve real customers.

## When You Need an External Portal

An external captive portal becomes the right choice when:

**You use Square POS and want Wi-Fi tied to purchases.** This is the clearest case. The Hotspot Manager has no Square integration. If you want guests to enter their receipt code to get online — automatically verifying that they're a paying customer without staff involvement — you need an external portal that talks to the Square API.

**You want a fully branded experience.** A captive portal is part of your customer's experience with your business. A generic UniFi portal page with a small logo doesn't match the care you put into your physical space. External portals can match your exact brand colors, fonts, and layout.

**You can't staff voucher management.** Generating, distributing, and managing vouchers is an ongoing operational task. At peak hours, the last thing your front-line staff should be doing is troubleshooting voucher codes. Purchase-based verification removes this entirely — the customer enters their own code.

**You need network monitoring alongside guest access management.** Some external portal services bundle network health monitoring, alerting, and expert support. If you want proactive visibility into your UniFi network without hiring a separate managed service, this is efficient.

**You have multiple locations.** Managing Hotspot Manager vouchers across several sites requires logging into each controller separately. A centralized external portal service manages all sites from a single dashboard.

## Security and Privacy Considerations

The Hotspot Manager stores connection data locally on your controller. No data leaves your network by default (unless you have UniFi Site Manager cloud access enabled). This is a reasonable privacy posture for operators who don't want any third-party involvement with their guest data.

External portals vary significantly in their data handling. Some are fundamentally marketing platforms — they collect guest email addresses, build profiles, and generate revenue by targeting your customers with advertising. Read the privacy policy carefully.

Captivi's approach is different: we collect only what's needed to validate receipt codes and manage sessions. No email collection, no marketing profiles, no data resale. If privacy is a priority — and for most hospitality operators, it should be — verify what your portal service does with the data it sees.

## Setup Complexity

**Hotspot Manager:** Built into the UniFi controller. Configure it in **UniFi Network → WiFi → [SSID] → Advanced → Guest Portal → Hotspot**. Basic setup takes 15 minutes. Voucher management is ongoing but straightforward.

**External portal (DIY):** Significant setup effort. You need a portal server, HTTPS certificate, backend code, UniFi API integration, and walled garden configuration. Ongoing maintenance includes firmware compatibility checks and certificate renewals. Realistic estimate for a competent developer: 2-4 days to build a functional portal.

**External portal (managed service like Captivi):** Setup is handled by the service provider. You provide your Square credentials, logo, and UniFi controller access. Onboarding typically takes a few days. After that, the system is self-managing. The ongoing operational cost is the monthly subscription fee.

## Cost Comparison

- **UniFi Hotspot Manager:** $0 additional cost (included with your UniFi controller software)
- **DIY external portal:** Development time + hosting costs (estimate $20–$50/month for a small VPS, plus your time)
- **Managed external portal (e.g., Captivi):** [Starting at $50/month](/pricing/) for a single location, plus a one-time setup fee

The Hotspot Manager is free, but "free" software always has a cost — in this case, staff time for voucher management and the limitations of what the tool can do. For a busy café that serves 100+ transactions per day, the operational overhead of voucher-based Wi-Fi adds up quickly.

## The Captivi Advantage

Captivi was built specifically for the gap that the UniFi Hotspot Manager doesn't fill: purchase-verified guest access for businesses that already use Square POS and UniFi gear.

What you get that the Hotspot Manager can't provide:
- Square POS receipt code verification — automatic, no staff involvement
- Fully branded portal matching your business identity
- No data harvesting, no marketing to your customers
- Network monitoring on higher tiers
- Expert support from professionals with real UniFi experience
- Multi-site management under a single account

What the Hotspot Manager still does better:
- Zero additional monthly cost
- No external service dependencies
- Full local data control

The right choice depends on your priorities. If purchase-verified access and a professional portal experience matter to your business, the Captivi upgrade is worth the monthly investment. If you just need basic guest network control and you're comfortable managing vouchers, the built-in Hotspot Manager is a capable tool.

See the complete feature breakdown on the [Captivi Features page](/feature/), or [view pricing](/pricing/) to find the right tier for your operation.

---

## Ready to Move Beyond Vouchers?

If you're running a hospitality business on UniFi with Square POS and you're tired of the voucher management overhead, [contact us](/contact/) to learn how Captivi can replace it with automated, purchase-verified guest access.
