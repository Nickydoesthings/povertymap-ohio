data sources:

U.S. Census Bureau — American Community Survey (ACS) 5-Year Estimates
measures official poverty rate

Bureau of Labor Statistics — Local Area Unemployment Statistics (LAUS)
measures unemployment rate

County Health Rankings & Roadmaps (University of Wisconsin Population Health Institute + RWJF)
measures food insecurity AND severe hoursing cost burden

---

* Section 1 — Setup & Config : imports, constants, etc.
* Section 2 — Data Acquisition : acquire the data, fill data/raw.
Section 3 — Cleaning & Standardization : join it all on 5-digit county FIPS codes.
Section 4 — Integration : one master table built on FIPS.
Section 5 — Normalization : some are %, some are $. Min-max scaling vs. percentile rank?
Section 6 — Composite Index : weight the sources differently.
Section 7 — Visualization : see it all come together.