# Data

No data files are included in this repository. Euromonitor and Statista data were accessed under Audencia's academic licence and can't be redistributed. These are the exports the report was built from.

| Folder | Source | Files | Content |
|---|---|---|---|
| `raw/passport_2026-10-02/` | Euromonitor International, Passport (licensed) | 6 `.xls` | Sportswear, retail value: market size (current and constant 2025 prices, fixed 2025 exchange rates, 2011–2030), brand and company shares (2016–2025), retail channels (2011–2025), sports apparel by demographic |
| `raw/statista_2026-10-02/` | Statista Market Insights (licensed) | 56 `.xlsx` | Footwear (total, Sneakers, Running, Gym & Training) and Sports & Swimwear, 2018–2031; Europe plus Germany, UK, France, Italy, Spain, Netherlands, Poland; EUR |
| `raw/google_trends_2026-10-02/` | Google Trends (public) | 28 `.csv` | Monthly search interest, Jan 2019–Sep 2026, 7 countries × 4 comparison sets |
| `raw/investor/` | adidas, Nike (SEC EDGAR) and Puma investor relations (public) | releases and reports | Quarterly Europe / EMEA net sales and currency-neutral growth, 2019–2026 |

Geographies: 12 Western European countries plus Poland, Czech Republic, Hungary and Romania. Türkiye is excluded everywhere, including from the Western Europe totals, because inflation inflates its euro values at fixed exchange rates (see `docs/methodology.md`).

## Google Trends comparison sets

Every set except `brands` contains "adidas Samba", which is used to rescale the sets onto one index per country (100 = Samba's peak month).

- `adidas_lifestyle`: adidas Samba, Gazelle, Spezial, SL 72, Taekwondo
- `rival_lifestyle`: adidas Samba, Nike Air Force 1, Nike Dunk, New Balance 530, Puma Speedcat
- `running`: adidas Samba, adidas Adizero, Nike Pegasus, On running, Hoka
- `brands`: adidas, Nike, Puma, New Balance, ASICS
