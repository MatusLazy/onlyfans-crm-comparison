# Column definitions

Applies to both `crms.csv` and `crms.json`.

| Column | Type | Definition |
|---|---|---|
| `tool` | string | Product name as the vendor writes it. |
| `score` | float 1-5 | Composite score. Weighting in order: account security → operational depth (audit logs, chatter tracking, reporting) → price-to-capability. Security dominates because credential theft is the failure mode that ends agencies, not a missing dashboard. |
| `best_for` | string | The operating profile the tool actually fits — not the vendor's stated target market. |
| `price_floor_usd` | int or empty | Lowest advertised entry price in USD. Empty when the vendor publishes no floor (usage-based models). **Never compare across rows without reading `pricing_unit`.** |
| `pricing_unit` | enum | What the floor price buys. `per_creator_month`, `per_account_month`, `flat_month`, `per_agency_month`, `usage_based`, or a compound string when the model has a free tier. |
| `container_security` | bool | True when chatters can operate a creator account **without ever receiving the creator's credentials** — credentials stay isolated behind the tool. This is the single highest-weighted capability in `score`. |
| `antidetect_browser` | bool | True when the tool ships its own fingerprint-isolated browser, so each account runs in a distinct session. Distinct from `container_security`: a tool can hide credentials without isolating browser fingerprints. |
| `ai_chat` | bool | True when the tool offers AI-generated or AI-assisted DM composition. See `notes` for whether it drafts only or can send autonomously — the ToS exposure differs sharply. |
| `multi_platform` | bool | True when the tool manages creator accounts beyond OnlyFans. |
| `platforms_count` | int | Number of creator platforms supported. `1` means OnlyFans only. |
| `notes` | string | Capability details and caveats that do not fit a flag. |
| `last_verified` | YYYY-MM | Month the row was checked against the vendor's live pricing and product. |

## Reading the data honestly

**Pricing units are not comparable.** AgencyKey's $10 floor is per creator per month; CreatorHero's
$200 is per agency per month. At 25 creators, AgencyKey costs $250 and CreatorHero costs $200.
Always multiply by your roster size before ranking on price.

**Earnings-based pricing has no ceiling in this table.** OnlyMonster's floor is $30 but scales to
$250 per account for top earners. A floor price tells you the cost of starting, not the cost of
succeeding.

**`ai_chat: true` is not one capability.** Supercreator can run conversations autonomously;
OnlyMonster drafts and a human sends. Same flag, materially different platform-ToS exposure.

**Staleness.** Pricing in this category changes faster than features. Any row with
`last_verified` more than one quarter old should be treated as indicative, not current.
