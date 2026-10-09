# Kada Dham – Sheetla Mata implementation plan

## Product shape

A Hindi-first devotional temple website with clean URL routing and a warm, editorial temple identity. Visitors can understand the temple, browse the documented puja catalog, view a YouTube search embed, and contact the documented priest number. Authenticated devotees can use the documented OTP flow, create a booking, open UPI/PhonePe payment links, manually confirm payment, view their own bookings, and update their display name.

## Design direction

**Design movement:** contemporary Indian devotional editorial — ivory paper, sandstone blocks, vermilion ink, saffron actions, and restrained gold details. **Core principles:** reverent rather than theatrical, useful before decorative, strong type hierarchy, and honest provenance for placeholder imagery. **Layout paradigm:** alternating editorial bands and asymmetric image/text compositions rather than a dashboard-like card wall. **Signature elements:** circular ॐ mark, fine gold rules, and vermilion/saffron action hierarchy. **Interaction:** clear next steps with low-noise hover states; no gratuitous motion. **Typography:** Playfair Display for devotional headlines and DM Sans for interface/content. **Brand essence:** a trustworthy digital doorway to Kada Dham for devotees who want to learn, offer a puja, and keep their booking details in one place. Personality: grounded, warm, clear. **Voice:** “भक्ति को एक सरल कदम बनाइए।” and “अपनी श्रद्धा के लिए एक सेवा चुनें।” The supplied temple logo and photos can replace the centralized placeholders without layout changes.

## Implementation

The frontend lives in `client/src/kada-app.jsx` and `client/src/styles.css`. It uses a small history-based router so every requested page has a genuine URL and browser refresh is supported by the Express SPA fallback. The backend in `server/index.ts` exposes the PRD routes, signs seven-day HMAC session tokens, validates ownership, snapshots puja name/price into bookings, and persists local state to a JSON file when MongoDB credentials are not available. `client/public/manus-routes.json` declares the complete page route set.

The implementation intentionally keeps the PRD’s current OTP mock response and manual payment confirmation visible as development/known limitations. It does not invent payment verification, official statistics, testimonials, or additional religious claims. The original image/logo replacement points are the `PLACEHOLDER_IMAGES` object in the frontend and the corresponding `image`/`temple.images` fields in the backend.
