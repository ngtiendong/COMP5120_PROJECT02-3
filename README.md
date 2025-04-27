# Final Proposal: Interactive Visualization of Race to Net Zero: Visualising the Energy-Emission

---

## 1  High-Level Goal  
**Create an interactive storyline that fuses global energy-use and GHG-emissions data, showing whether today’s shift toward renewables is fast enough to keep the world on a Net-Zero-by-2050 pathway.**

---

## 2  Description & Motivation  
Despite record growth in solar & wind, global primary-energy demand and greenhouse-gas (GHG) emissions both continue to climb.  Public discourse is littered with cherry-picked statistics that hide the real question:  

> *Are renewables expanding rapidly enough to offset economic and population growth while still cutting absolute emissions?*

This project merges two complementary sources—**World Energy Consumption** (multi-fuel, multi-metric time series) and **Climate Watch Historical Emissions** (full GHG inventory)—to reveal the carbon intensity of each region’s energy system from 1990 onward.  Users will be able to explore whether the pace of decarbonisation aligns with the Intergovernmental Panel on Climate Change (IPCC) and International Energy Agency (IEA) milestones for Net Zero 2050.  The visual narrative will aid students, educators and policy analysts in understanding where progress is real and where ambition gaps remain.

---

## 3  Dataset Details  

### 3.1  World Energy Consumption (Kaggle)  
* **Source & Access**  <https://www.kaggle.com/datasets/pralabhpoudel/world-energy-consumption> – downloadable ZIP containing multiple CSVs.  
* **Temporal Coverage**  Mostly 1965 – 2023 (latest Energy Institute Statistical Review).  
* **Spatial Coverage**  Global, 200+ ISO-3 countries + aggregates (World, EU-27, OECD, Asia Pacific, etc.).  
* **Key Variables**  
  * **Consumption (TWh)** – `primary_energy_consumption`, `renewables_consumption`, `fossil_fuel_consumption`, `solar_consumption`, `wind_consumption`, `hydro_consumption`.  
  * **Electricity Generation (TWh)** – `electricity_generation`, `renewables_electricity`, `solar_electricity`, `wind_electricity`, `hydro_electricity`.  
  * **Shares (%)** – `renewables_share_energy`, `renewables_share_elec`, `fossil_share_energy`, `fossil_share_elec`.  
  * **Per-Capita (kWh per person)** – `energy_per_capita`, `renewables_elec_per_capita`.  
  * **Environmental** – `carbon_intensity_elec` (g CO₂-eq/kWh).  
  * **Socio-Economic** – `population`, `gdp`.  
* **Reasons for Selection**  
  * Ready-made, well-documented CSV (no scraping required).  
  * Multi-dimensional: fuel type, shares, per-capita and intensity—all ideal for layered visualisations.  
  * Covers Vietnam explicitly, enabling regional spotlights.

### 3.2  Climate Watch – Historical Emissions  
* **Source & Access**  Public API by World Resources Institute (WRI) – <https://www.climatewatchdata.org>.  
* **Collection Method**  
  * Python script queries  
    ```text
    /api/v1/data/historical_emissions
        ?regions[]=<ISO3>
        &gas_ids[]=473     # All GHG (or 474 CO₂, 475 CH₄, …)
        &sector_ids[]="" # Total incl. LUCF
        &source_ids[]=""  # Climate Watch (CAIT)
    ```  
  * The script loops over pages, saves raw JSON, “explodes” `{year,value}` objects, and exports  
    * `climatewatch__emission.csv` – tidy long (one row per year × gas), and  
* **Temporal Coverage**  1850 – 2022 (varies by gas, full All-GHG series 1990 – 2022 for most countries).  
* **Variables**  `iso_code3`, `country`, `year`, `gas`, `sector`, `unit` (= Mt CO₂-eq), `value_mtco2e`.  
* **Reasons for Selection**  
  * Complements the energy dataset with directly comparable emission quantities.  
  * Standard ISO-3 codes align cleanly with Kaggle dataset keys.  
  * Free, automatically updateable; script can be re-run to pull newer years or more gases.

---

## 4  Why This Problem?  
Net-Zero 2050 is the core benchmark of climate policy.  Visualising real-world progress—grams of CO₂-equivalent per kilowatt-hour, renewable share growth, and emission-reduction pace—turns abstract pledges into measurable gaps.  From a data-visualisation standpoint, the task is rich: two data sources, ∼40 variables each, thousands of rows, multiple units and scales—all wrangled and communicated entirely in **R**.

---

## 5  Research Questions  

* **RQ 1 – Renewable Share Trajectory**  
  *How quickly has the share of renewables in primary energy grown since 2000, and which regions are on track to cross the 60 % renewables threshold before 2050?*

* **RQ 2 – Renewable Growth vs Emission Decline**  
  *For the ten largest emitters, does the average annual growth rate of renewable electricity generation (2010–2023) exceed the annual GHG-reduction rate required to hit Net-Zero by 2050?*

---

## 6  Weekly Plan (4-week sprint)  
| Week | **Dong** | **Minh** | Deliverable |
|------|----------------------------|--------------------------------|-------------|
| 1 | • Design repo structure<br>• Download Kaggle dataset<br>• Write & debug `crawl_climatewatch.py`<br>• Draft data dictionary skeleton | • Verify ISO-3 country codes & variable names<br>• Collect API doc links & licence notes | `/data/raw/` populated; working crawler; initial data-dictionary .md |
| 2 | • Develop `01_clean.R` (tidy, derive carbon-intensity & renewable shares)<br>• Push processed CSVs | • Run QA checks on missing values & units<br>• Create quick EDA plots for sanity check | `/data/processed/*.csv`; `explore.qmd` with validation plots |
| 3 | • Compute metrics for RQ 1 & 2 (dplyr)<br>• Build core ggplots (line, slopegraph, small-multiple)<br>• Refine colour palette & annotation | • Test charts for readability on mobile & dark-mode<br>• Draft narrative captions | `analysis.qmd` with polished static charts & captions |
| 4 | • Assemble Shiny/Quarto dashboard (filters, Net-Zero summary panel)<br>• Write final report/proposal PDF | • Style dashboard UI, add accessibility labels<br>• Proof-read report & references | Deployable dashboard folder; `proposal.pdf` ready for submission |


---

## Additional Notes
- **New Skill**: Advanced Shiny app development, including `plotly` for interactive visualizations and `shiny` for dashboard creation, not covered in class.
- **Accessibility**: Visualizations will use colorblind-safe palettes (via `colorblindr`) and high-contrast designs, aligning with WCAG guidelines.
- **Reproducibility**: All scripts (data cleaning, visualization, app development) will be in R, documented in Quarto, and hosted on GitHub.

This proposal ensures a reproducible, challenging project distinct from prior work, leveraging a complex dataset to create an impactful visualization tool for Vietnam’s energy transition.