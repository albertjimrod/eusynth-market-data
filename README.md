# European Synthesizer Market Observatory — Open Dataset

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

Aggregate price statistics for second-hand synthesizers collected from European online marketplaces. Published by the [European Synthesizer Market Observatory](https://intellisynthprices.com) — an independent, non-commercial open data project.

> **No seller data is included.** Usernames, names, and contact details are never collected or stored. See our [privacy policy](https://intellisynthprices.com/about/legal/privacy) and [scraping policy](https://intellisynthprices.com/about/bot).

---

## Files

### `data/fair_prices.csv`
Fair Market Price estimates for 122 synthesizer models with sufficient market activity.

| Column | Description |
|---|---|
| `manufacturer` | Brand (e.g. Korg, Roland, Moog) |
| `model` | Model name (e.g. Minilogue XD) |
| `canonical_name` | Full canonical name (e.g. Korg Minilogue XD) |
| `price_p25_eur` | 25th percentile — lower end of market range |
| `price_p50_eur` | Median — most typical asking price |
| `price_p75_eur` | 75th percentile — upper end of market range |
| `sample_size` | Number of listings used in the calculation |
| `window_days` | Rolling window (90 days) |
| `sources` | Marketplaces included in this estimate |
| `computed_date` | Date the estimate was last calculated |

### `data/products.csv`
Canonical product catalogue of 3,037 synthesizer models tracked by the Observatory.

| Column | Description |
|---|---|
| `manufacturer` | Brand |
| `model` | Model name |
| `canonical_name` | Full canonical name |
| `total_listings` | Total listings observed across all sources (excl. eBay) |
| `listings_with_price` | Listings with a valid EUR price |
| `first_seen` | Date of first listing observed |
| `last_seen` | Date of most recent listing observed |
| `has_fmp` | 1 if a Fair Market Price is available for this model |

### `data/monthly_market_stats.csv`
Monthly aggregate activity by marketplace source.

| Column | Description |
|---|---|
| `month` | Year-month (YYYY-MM) |
| `source` | Marketplace name |
| `source_country` | Primary country of the marketplace |
| `listings_scraped` | Total listings collected that month |
| `listings_with_price` | Listings with a valid EUR price |
| `avg_price_eur` | Average asking price |
| `min_price_eur` | Minimum asking price |
| `max_price_eur` | Maximum asking price |
| `distinct_products` | Number of distinct canonical products observed |

---

## How to cite

```
European Synthesizer Market Observatory. (2026). European Synthesizer Market
Observatory — Open Dataset [Data set].
https://github.com/albertjimrod/eusynth-market-data
License: CC BY 4.0
```

Or use the `CITATION.cff` file included in this repository.

---

## Methodology

Data is collected approximately every hour from four public second-hand marketplaces: **Hispasonic** (ES), **Soundsmarket** (ES), **Audiofanzine** (EU), and **Noiz** (GR). Each listing undergoes a 72-hour embargo before being processed. Listing titles are matched to canonical product names using an ML classifier. Fair Market Prices are calculated as percentile statistics over a 90-day rolling window (minimum 5 samples, outliers excluded).

Full methodology: [intellisynthprices.com/about/methodology](https://intellisynthprices.com/about/methodology)

---

## Licence

## 📊 Data Ownership & Usage Policy

### 🔹 Raw Listing Data
All individual listing content — including titles, descriptions, prices, images, URLs, seller information, and marketplace metadata — is sourced from the following third-party marketplaces across Europe:

- **Hispasonic** (hispasonic.com)
- **Noiz** (noiz.es)
- **Audiofanzine** (audiofanzine.com)
- **Soundsmarket** (soundsmarket.com)


**This data remains the exclusive property of its original sources.** IntelliSynthPrices.com acts solely as an aggregator and visualization layer: we index, normalize, and display publicly available information to help users discover fair deals. We do not claim ownership, copyright, or exclusive rights over this raw content.

### 🔹 Derived Analytics & Aggregations
Any **original analysis, statistical models, or derived insights** produced by IntelliSynthPrices (including percentile calculations P25/P50/P75, "fair price" indicators, trend forecasts, rarity scores, and permanence distributions) are released under:

[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

✅ You are free to use, share, and adapt these **derived works** for any purpose — including commercially — provided you:
- Give appropriate credit to **IntelliSynthPrices / European Synthesizer Market Observatory**
- Link to the original source where feasible
- Indicate if changes were made

### 🔹 Commercial Use of Aggregated Data
If you intend to use **any data displayed or aggregated by IntelliSynthPrices** for commercial, economic, or business purposes (including but not limited to: resale, API services, competitive intelligence, or product integration), you must:

1. **Notify and seek permission from the original marketplace(s)** that own the underlying listing data:
   - Hispasonic → contact via [hispasonic.com/contact]
   - Noiz → contact via [noiz.es/contacto]
   - Audiofanzine → contact via [audiofanzine.com/contact]
   - Soundsmarket → contact via [soundsmarket.com/contact]


2. **Credit IntelliSynthPrices** for any derived analytics or value-added insights used

3. **Comply with the terms of service** of each source marketplace

> ℹ️ IntelliSynthPrices does not broker data licensing agreements. It is your responsibility to contact original sources directly for usage rights.

### 🔹 Fair Use & Non-Commercial Research
Academic researchers, journalists, and non-commercial projects may use aggregated statistics or anonymized trends from this platform under fair use principles, provided:
- No personal or seller-identifiable information is republished
- Source marketplaces are explicitly acknowledged (e.g., *"Data sourced from Hispasonic, Noiz, Audiofanzine via IntelliSynthPrices"*)
- IntelliSynthPrices is credited for derived methodology

### 🔹 Attribution Examples
✅ Correct attribution for derived analytics:
> *"Price insights powered by IntelliSynthPrices (European Synthesizer Market Observatory). Raw listings sourced from Hispasonic, Noiz, and Audiofanzine."*

✅ Correct attribution for non-commercial research:
> *"Market analysis based on aggregated data from IntelliSynthPrices.com, indexing public listings from Hispasonic, Noiz, Soundsmarket, Audiofanzine, Thomann B-Stock, and eBay Kleinanzeigen."*

### 🔹 Contact & Clarification
Unsure about your use case? Reach out before proceeding:
📧 infol@intellisynthprices.com]  
🌐 https://intellisynthprices.com/contact


---

## Updates

This dataset is updated periodically. Watch this repository to be notified of new releases.

For questions, corrections, or takedown requests: [contact@intellisynthprices.com](mailto:contact@intellisynthprices.com)
