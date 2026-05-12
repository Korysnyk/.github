# Korysnyk

AI-powered platform for analysis of food, cosmetics and household chemicals via barcode scanning. Mobile app for iOS and Android, web admin dashboard, multi-tenant backend.

*The name derives from Ukrainian "корисний" (useful) plus the suffix "-nyk", literally a helper in making beneficial choices.*

Current status: public beta on the Ukrainian market, preparing for expansion to European markets.

Website: https://korysnyk.care
API: https://api.korysnyk.care
Admin: https://dashboard.korysnyk.care

## Overview

Korysnyk lets consumers scan a barcode and receive instant analysis of a product, covering ingredient safety, nutritional value and personal compatibility with the user's health profile. The platform supports three product categories: food, cosmetics, household chemicals.

Analysis is performed at two levels.

- Lumi, general analysis of composition, cached by EAN13, available for any user
- Swapr, personal recommendations based on the user's health profile, cached by the sha256 hash of the profile

## Features

Barcode scanner

- EAN-13, EAN-8, UPC-A, UPC-E with checksum validation
- Voice and text search
- Submission of packaging photos for moderation when a product is missing from the database
- Per-user scan history persisted server-side

Product analysis

- Overall safety score 0 to 100 with color indicator
- Safety level classification, high / medium / low
- Nutri-Score grade A to E
- NOVA group for food processing level
- Positive, negative and missing nutrients
- Text warnings for specific ingredients
- INCI standard for cosmetics, safety profile for household chemicals

Personalization

- Health profile with allergies, chronic conditions, dietary preferences, skin type
- Automatic allergen warnings during scanning
- BMI computed server-side
- Recommendations recalculated only when the profile actually changes

AI assistants

- Savora, culinary expert (recipes, ingredient substitution, allergen warnings)
- Medic, nutrition advisor (cardiovascular impact, glycemic index, drug-food interactions)
- Cosmo, expert on cosmetics and household chemicals (INCI analysis, skin type guidance, safety for pregnant and nursing users)

## Technology Stack

Backend

- Python, Litestar 2 on uvicorn
- SQLAlchemy 2 async with asyncpg, PostgreSQL 15+
- Atlas for schema migrations
- MongoDB for raw product variants
- Meilisearch for text search and autocomplete
- Qdrant with embeddings for semantic search and similar products
- OpenRouter as the single LLM gateway, `google/gemini-2.5-flash-lite` by default

Mobile client

- Flutter for iOS and Android
- Dio with a JWT interceptor, automatic refresh on 401 with the `TOKEN_EXPIRED` marker
- Riverpod for state, SharedPreferences for token storage
- Single profile synchronized across mobile and web

Web admin

- Submission moderation, role and limit management

Security and reliability

- JWT with separate access and refresh tokens, proactive refresh five minutes before expiry
- werkzeug password hashing
- Soft delete with a 30-day restore window
- CORS with wildcard subdomain support
- GlitchTip (Sentry-compatible) for error tracking

## Action Limits

| Action          | Type                       | Free plan       |
| --------------- | -------------------------- | --------------- |
| scan            | barcode scan               | 40 / day, 90 / week |
| chat_create     | new chat                   | 2 / week        |
| chat_message    | chat message               | 40 / week       |
| product_submit  | submission for moderation  | 40 / day, 80 / week |

Premium tiers `premium_2x`, `premium_5x`, `premium_10x` multiply the base limits by the corresponding coefficient.

## Privacy

Health data is sensitive by design.

- Data is stored only on Korysnyk servers, no transfer to third parties
- Soft delete with a 30-day restore window, then complete erasure
- JWT-based authentication with separate access and refresh tokens
- GDPR-aligned account deletion flow

## Repositories

Source code is kept in private repositories. The public organization page hosts marketing materials, legal documents and limited supporting artifacts only. API access for third-party developers is planned, contact the team for details.

## Languages

Ukrainian and English. The AI assistants understand both languages; coverage of products on the Ukrainian market is more complete in the Ukrainian dataset.

## License

Proprietary. All rights reserved. See [LICENSE](https://github.com/Korysnyk/.github/blob/main/LICENSE) for the full text.

## Contacts

- Support, partnerships and licensing: admin@korysnyk.care
- Author: Yurii Kachaniuk, Telegram [@trimbler](https://t.me/trimbler)
- Legal entity: Korysnyk LTD, England and Wales
- Company number: 16942523
- Registered office: 128 City Road, London, United Kingdom, EC1V 2NX

---

Copyright (c) 2025-2026 Korysnyk LTD. All rights reserved.
