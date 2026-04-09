---
date: 2026-04-09T00:00:00Z
title: How Breweries Use Guest Wi-Fi to Drive Repeat Square Sales
seo:
  page_description: Discover how craft breweries connect their UniFi guest Wi-Fi to Square POS — turning every Wi-Fi login into a touchpoint for loyalty and repeat visits.
  canonical_url:
  featured_image:
  author_twitter_handle:
  open_graph_type: article
  no_index: false
categories:
  - Square POS
  - WiFi
  - Marketing
author: Captivi Team
thumbImg:
  image_path: /images/blog/blog-thumb-6.jpg
featuredImg:
  image_path: /images/blog/featured-image-2.jpg
draft: false
---

Craft brewing is a relationship business. Regulars aren't just revenue — they're the audience for your seasonal releases, the evangelists who bring their friends on a Friday night, and the people who actually care about the story behind your Double IPA. Building those relationships requires more than great beer; it requires intentional touchpoints.

Guest Wi-Fi is one of those touchpoints, and most breweries are wasting it.

## Why Brewery Wi-Fi is Different

Breweries aren't coffee shops. The dwell time is longer — guests often stay two to four hours for a flight, a meal, and conversation. Weekend taprooms fill up with groups who are genuinely there to experience the place, not just caffeinate before a meeting.

This extended dwell time changes the math on guest Wi-Fi. A coffee shop customer might connect for 45 minutes. A brewery guest might connect for three hours across two or three purchases. They're not just using your Wi-Fi — they're living in your brand's environment for an entire afternoon.

That makes every Wi-Fi session a window for meaningful engagement: not spam, not pop-up advertising, but a simple, well-designed moment to invite them back.

The second distinction is the purchase pattern. Most taproom guests make multiple transactions in a single visit — a flight to start, a pint for the second round, maybe a growler fill on the way out. If your guest Wi-Fi is tied to Square purchases, every transaction can refresh or extend access, creating a natural rhythm between buying and connecting that reinforces the visit experience.

## The Missed Opportunity: Wi-Fi Without Engagement

The standard brewery Wi-Fi setup looks like this: an open SSID with a password scrawled on a chalkboard near the entrance. Anyone within range can connect — customers, people in the parking lot, staff on break, delivery drivers. The network is a shared resource with no relationship to the business running it.

This is a missed opportunity in at least three ways:

1. **You have no idea who your regulars are at a network level.** When a customer visits six times in two months, your Square account might know (if they use the same card), but your Wi-Fi network treats every visit as anonymous.

2. **Your brand is absent from the connection experience.** The "enter password" prompt has nothing to do with your taproom, your beers, or your story. It's dead air in a moment when the customer is paying attention.

3. **You're giving away infrastructure value with no return.** Wi-Fi costs real money — hardware, internet service, ongoing maintenance. It's a customer benefit. If it's completely unconditioned, it becomes an expected utility rather than a perk of being a paying guest.

## Purchase-Gated Wi-Fi: Earn Your Connection

Captivi introduces a simple mechanic that changes the equation: guests enter their Square receipt code to access Wi-Fi. This does several things simultaneously.

First, it ties network access to actual purchases. Only people who have bought something get online. Freelancers who wander in to use your Wi-Fi for free — a real problem for well-located urban taprooms — are quietly redirected.

Second, it creates a branded moment. When a guest connects to your network, they don't see a generic password prompt — they see your taproom's logo, your colors, and a clean interface that feels like part of the experience. That's a small but real brand impression that builds over multiple visits.

Third, it gives you session control. Access sessions can be time-limited or purchase-limited. If you want to grant two hours per order, or extend access automatically when the guest makes a second purchase, those are configurable parameters. The network reflects how your taproom actually works.

Some brewery owners worry that friction at the Wi-Fi stage will frustrate customers. The reality is that entering a five-digit order code takes about ten seconds — and customers are already holding their receipt. The friction is small, the perception of fairness is high (you bought something, you get Wi-Fi), and the experience is significantly more polished than "the password is hops2024."

## Turning Wi-Fi Logins into Loyalty Touchpoints

Here's where the real opportunity lies. The captive portal — the page that appears when a guest connects — is a prime piece of real estate that most brewery operators leave blank.

A well-designed Captivi portal can surface:

- **A newsletter signup prompt.** A simple "Get notified about new releases and taproom events" message with a single email field captures opt-ins at exactly the right moment — when the customer is physically in your space, enjoying a pint, and feeling good about your brand. This isn't a pop-up ad. It's a contextually appropriate invitation.
- **Upcoming events.** Your taproom calendar, featured releases, live music nights, or trivia events can be embedded directly in the portal page. A guest who just discovered your taproom this Saturday becomes a potential returnee next Saturday if you tell them what's coming.
- **Loyalty program enrollment.** If you run a punch card, stamp card, or digital loyalty program through Square or a third-party app, the portal is the right moment to mention it. "Join our Mug Club — tap here to learn more" is a natural call-to-action for a guest who's already committing to a visit.
- **Growler and merchandise promotion.** "Taking something home? Growler fills are available at the bar." Small, contextually relevant prompts that drive incremental transactions.

None of these require you to harvest personal data or build customer profiles. Captivi doesn't collect email addresses on your behalf — that data goes to your mailing list service (Mailchimp, Klaviyo, whatever you use). The portal is a bridge, not a database.

## UniFi + Square: The Technical Stack

Captivi is built specifically for this hardware combination, and it's worth understanding why it works so well.

**UniFi** is the dominant enterprise-grade networking platform in the small-to-medium business segment. Ubiquiti's equipment — Dream Machines, Cloud Gateways, access points like the U6 Pro — provides the kind of network stability and segmentation that a busy taproom needs. The External Guest Portal API that UniFi exposes is what makes purchase-gated captive portals possible without additional hardware.

**Square** is the POS of choice for most independent food and beverage operations precisely because it handles complex ordering (food menus, modifiers, discounts) without requiring expensive POS software licensing. Its API is well-documented and actively maintained, which is why Captivi can validate receipt codes in real time without significant latency.

The combination is common enough — and underserved enough — that building a tight integration between the two is genuinely useful. Most generic captive portal software treats POS integration as an afterthought. For Captivi, it's the entire product.

## Captivi's Role: The Integration Layer

Captivi sits between your UniFi controller and your Square account. When a guest enters their receipt code on the portal, Captivi validates it against the Square API, then signals your UniFi controller to authorize that client's MAC address for the configured session duration.

You don't manage this process. Once configured, it runs entirely automatically. There's no daily task, no voucher generation, no staff password resets. The portal handles everything.

What you do get is access to the Captivi dashboard, where you can see active sessions, review connection history, and adjust configuration settings. If your controller has monitoring enabled (available on the Monitored and Managed tiers), you'll also get alerts for network issues before they affect your taproom.

See the complete breakdown of what Captivi handles on the [Features page](/feature/).

## Real-World Scenario: A Taproom on a Friday Night

It's 7 PM on a Friday. Your taproom is busy — a mix of regulars and first-timers. Here's how Captivi changes the experience:

- **6:55 PM:** A group of four sits down, orders a flight. One person pulls out their phone, sees your guest SSID, and connects. The branded portal appears with your logo.
- **7:00 PM:** They enter the order code from the receipt. All four devices are authorized (they're on the same network segment). Access granted for three hours.
- **7:05 PM:** On the portal confirmation page, they see an event listing: "Cask Night next Friday — limited seating." Two of them note it.
- **8:30 PM:** One guest orders another round. That new receipt code resets the session clock — they're set for the rest of the evening.
- **9:15 PM:** Near closing, a first-time visitor uses the portal newsletter prompt to sign up for your email list. Three days later, they get your release announcement and share it with a friend.

This is the compounding value of connected Wi-Fi. Not every session drives an action. But over dozens of Friday nights, the cumulative effect on awareness, repeat visits, and word-of-mouth is meaningful.

## Pricing

Captivi's pricing is straightforward and scales with what you need:

- **Secured ($50/month):** Core portal with receipt verification and branded design. Perfect for a taproom that wants purchase-gated Wi-Fi without any IT overhead.
- **Monitored ($100/month):** Adds UniFi network monitoring and chat support. If you want proactive visibility into your network health, this is the tier to consider.
- **Managed ($250/month):** Full-service management, phone support, proactive recommendations, and CloudRoute for ISP-agnostic setup. Built for multi-tap operations or breweries that don't want to think about IT at all.

A one-time setup fee of $250 covers portal configuration, Square integration, and UniFi setup. This fee is waived for Managed tier customers.

Full pricing details on the [Captivi Pricing page](/pricing/).

---

## Ready to Connect Your Taproom?

If you're running a brewery with UniFi gear and Square POS, you have the infrastructure for a significantly better guest Wi-Fi experience — one that reinforces your brand, encourages repeat visits, and stops non-customers from free-riding your network.

[Get in touch to start the conversation](/contact/). Setup is handled by the Captivi team, so there's no technical lift on your end.
