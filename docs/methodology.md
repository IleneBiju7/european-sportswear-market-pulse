# Data & Methodology: European Sportswear Market Pulse

**Question:** adidas has closed the gap on Nike in Europe. Where can it keep winning in 2027, and where is the momentum at risk?

## 1. Sources

| Source | What | Coverage | Access |
|---|---|---|---|
| Euromonitor International, Passport | Sportswear market size (nominal and constant 2025 prices), brand and company shares, retail channels, apparel by demographic | 12 Western European countries, Poland, Czech Republic, Hungary, Romania; 2011–2025 actual, 2026–2030 forecast | Licensed via Audencia, not redistributed |
| Statista Market Insights | Footwear (total, Sneakers, Running, Gym & Training) and Sports & Swimwear apparel: revenue, volume, price, online share | Europe plus 7 countries; 2018–2031 | Licensed via Audencia, not redistributed |
| Google Trends | Monthly search interest for 18 franchise and brand terms | 7 countries, Jan 2019–Sep 2026 | Public |
| Company filings | adidas, Nike and Puma quarterly results (Europe/EMEA net sales and currency-neutral growth) | Q1 2019–Q2 2026 (Nike to Q1 FY27) | Public |

## 2. Preparation

Raw exports were reshaped from wide (one column per year) to long format (one row per entity, country and year) in Python (pandas). The cleaning reruns end to end and validates its output every time.

## 3. Issues found and how they were handled

1. **Türkiye inflated the regional total.** At fixed 2025 exchange rates, Türkiye's current-price market grew 21× from 2016 to 2025 because of inflation, rising from 0.4% to 10% of "Western Europe" by 2030. It was removed and a derived *Western Europe excl. Türkiye* total was built. Regional 2019–25 CAGR: 3.3% vs 4.1% with Türkiye included.
   The same distortion affects the regional brand, company and channel shares. Türkiye's weight rose from 0.5% of regional sales in 2019 to 5.3% in 2025, and adidas holds 24% of Türkiye. Left in, Türkiye would flatter adidas's 2025 share (12.9% vs 12.3%) and its consumer-spend growth (+8.4% vs +5.5% in 2025). So those tables also get derived *excl. Türkiye* rows: region value minus Türkiye value, divided by the region market minus Türkiye's. Türkiye brands the region doesn't list separately come out of "Others". The report uses these rows for every "Western Europe" figure.
2. **Nested channel hierarchy.** Euromonitor channels nest (Retail → Offline → Non-Grocery → Leisure → Sports goods stores), so summing every row double-counts. Each channel was tagged with its level, its parent and an `is_leaf` flag. Only leaf channels add up to 100%. Leaves were grouped into 6 reporting buckets.
3. **Ownership changes split brand series.** 19 brands appear under more than one owner name. Some are real owner changes: Puma (Kering → Puma SE, 2018), Reebok (adidas → ABG, 2022), Champion (Hanesbrands → ABG) and Jack Wolfskin. Others are renames, such as Amer Sports Oyj → Amer Sports Inc for Salomon. After confirming no year has both owners, each brand was merged into one series with a year-specific `owner` column.
4. **"-" means different things.** In brand data it means "not ranked" (unknown), so those rows were dropped. In channel data it means no sales, so it was set to 0.
5. **Inflation.** Constant-price (2025) market sizes were added next to nominal. Real growth is the basis for all market-size growth claims. Statista forecasts and company growth rates are nominal and are labelled as such.
6. **Units and currencies.** Statista values were normalised to EUR million (some files are in billions). Statista converts at each year's exchange rate and Euromonitor at fixed 2025 rates, so the two sources are compared on growth rates and splits, never on euro levels.
7. **Google Trends scaling.** Each comparison set is indexed to its own peak. All sets except the brand-level one share "adidas Samba" as an anchor term, which was used to rescale them onto one scale per country. Indices are not compared across countries. "<1" was set to 0.5.
8. **Flagged, not adjusted.** Netherlands brand data is thin (70% "Others"). Puma's Austria share breaks in 2018 when its owner changed, likely a reclassification. Statista brand shares cover 2022 only (not used). Passport has no footwear vs apparel split for Sportswear (Statista used instead).

## 4. Validation (automated on every run)

- Brand, company, channel-leaf and demographic shares sum to 100% ±1.5 points for every geography and year, including the derived excl.-Türkiye rows (observed 99.5–100.7, Euromonitor rounding).
- No duplicate keys. Türkiye absent from all outputs.
- Spot checks against the source exports (e.g. adidas 12.9% share in Euromonitor's Western Europe total, 2025; 12.3% once Türkiye is taken out).
- Every number and claim in the report was recomputed from the clean data with the same logic as the DAX measures (audit, 11 Oct 2026).

## 5. Key findings from exploratory analysis

All "Western Europe" figures exclude Türkiye.

1. **adidas has nearly closed the gap with Nike in Western Europe:** Nike's lead fell from 4.8 points (2023) to 1.3 points (2025). adidas leads in 11 of the 16 countries, up from 6 in 2023.
2. **No real market growth:** Western Europe grew 3.3% a year nominally but 0.0% a year in real terms from 2019 to 2025.
3. **Germany is contracting:** −2.2% a year in real terms over 2019–25, and Euromonitor forecasts only +0.5% a year to 2030. Puma's share there doubled (3.6% → 7.6%).
4. **The UK is the largest market and Nike's stronghold:** €12.7bn. Nike leads by 6.2 points, its widest lead. One share point is worth about €127m.
5. **The terrace cycle has peaked:** Samba, Gazelle and Spezial searches peaked in 2024–25 in all 7 countries and are below peak everywhere. In the UK, Jan–Sep 2026 searches are down 51% for Gazelle and 23% for Samba on the same months of 2024. Nike Air Force 1 and Dunk have declined sharply.
6. **Running is the next battleground:** running/performance challengers rose from 4.6% to 6.9% share (2016–25). On, Hoka and Adizero searches are up on last year in all 7 countries (Jan–Sep 2026). Statista forecasts running shoes to grow 3.7% a year to 2030, faster than footwear overall (2.9%).
7. **Online stayed high after COVID:** online share jumped from 20.6% (2019) to 30.9% (2020) and reached 31.6% in 2025. Germany is at 46%, where store sales fell from €7.3bn to €6.3bn.
8. **Sell-in ran ahead of sell-out:** adidas's reported Europe sales grew 19.8% in 2024, while Euromonitor's adidas retail value grew 9.5%, consistent with retailers restocking. In 2025 the gap narrowed: +7.7% vs +5.5%.

## 6. Limitations

- Euromonitor and Statista figures are estimates of retail (consumer) value, not company revenue. They are compared with reported sales on growth, not levels.
- Search interest measures attention, not purchases.
- With internal data (POS, consumer panel, sell-in by account), each finding would be validated before acting on it.

## 7. Data licensing

Euromonitor and Statista data are used under Audencia's academic licence and are **not included** in the public repository. The licensed inputs and outputs, and the Power BI file, are not published.
