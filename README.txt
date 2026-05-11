Cryomatt Carbon Comparator V10

V10 changes:
- Electricity prices recalibrated for all countries/states in the database.
- Internal reference currency remains EUR/kWh.
- EUR, USD and local currency are display options only.
- France corrected to 0.20 EUR/kWh.

Sources used as calibration basis:
- Eurostat household electricity price statistics 2025 for Europe
- EIA average electricity price by state for USA
- GlobalPetrolPrices 2025 Q3 / Q1 2026 household electricity price database for global countries
- National tariff references where appropriate

Upload all files/folders to the GitHub Pages repository root.

V11 changes:
- Added optional client override fields:
  • Client LN₂ Carbon Footprint kgCO₂/L
  • Client Electricity Price /kWh
  • Client Electricity Carbon Footprint kgCO₂/kWh
- If a client field is empty, the calculator uses the database value.
- If a client field is filled, it overrides the database value for the calculation.

V12 update:
- Electricity Carbon Footprint kgCO2/kWh recalibrated using 2025 electricity mix carbon intensity logic.
- France corrected to 0.020 kgCO2/kWh based on RTE 2025 direct generation average of 19.6 gCO2e/kWh.
- Country values follow Ember/OWID 2025 electricity carbon intensity where available.
- US state values use EIA/state generation mix references.
- Client override fields remain active: if filled, client values override database values.

V13 update:
- Applied the Europe-style "Database & Client Values" design to every continent page.
- Database value is shown first, readonly.
- Client override value is shown directly underneath.
- If client override is empty, database value is used.
- If client override is filled, client value is used.
- V12 electricity CO2 recalibration is preserved.

V14 update:
- Removed the long country/price/source text displayed under the input fields.
- Source data remains inside the database and JavaScript objects.
- Calculator display is cleaner for customer demos.

V15 update:
- Fixed the Annual Cost Saving KPI.
- Previous logic compared total annual costs of systems with different box capacities.
- New logic uses an equivalent capacity basis:
  Annual Saving on Fusion Capacity = (Conventional Cost per Box - Fusion Cost per Box) × Fusion Boxes
  Annual CO2 Avoided on Fusion Capacity = (Conventional CO2 per Box - Fusion CO2 per Box) × Fusion Boxes

V16 update:
- Replaced box-based comparison with 2 ml vial-based capacity model.
- New capacity fields:
  • Conventional Standard Racks
  • Conventional Small Racks
  • Conventional Boxes per Rack
  • Fusion Standard Racks
  • Fusion Small Racks
  • Fusion Boxes per Rack
- Standard box = 100 vials.
- Small box = 25 vials.
- Total vials = (standard racks × boxes/rack × 100) + (small racks × boxes/rack × 25).
- Results now show cost per vial and CO2 per vial.
- Annual savings are calculated on Fusion total vial capacity.

V17 update:
- Fixed display of very small cost-per-vial values.
- Cost per vial and saving per vial now show 4 to 6 decimal places.
- Annual savings remain displayed as normal currency.

V18 update:
- CO2 per vial now displayed in grams (gCO2/vial) instead of kgCO2/vial.
- Improves readability for very small per-vial values.

V19 update:
- Fixed JavaScript calculation bug left from the old boxes model.
- Removed remaining boxesClassic/boxesFusion calculation references.
- Results now correctly use total vials from rack configuration.
- CO2 per vial displayed in gCO2/vial.

V20 update:
- Reordered all fields for better UX.
- New order:
  1. Country / Currency
  2. INPUT ENERGY DATA
  3. STORAGE CAPACITY
  4. DATABASE & CLIENT VALUES
  5. RESULTS

V21 update:
- Fixed JavaScript bug after field reordering.
- Restored hidden countryInfo element required by updateCountryPrice().
- Added safer checks before writing optional info fields.

V22 update:
- Fixed alignment of two-column fields in STORAGE CAPACITY.
- Labels now have consistent height so Standard Racks and Small Racks inputs align cleanly.

V23 update:
- STORAGE CAPACITY section split into two clear subsections:
  • CONVENTIONAL FREEZER
  • FUSION
- Cleaner comparison layout for customer presentations.

READY TO UPLOAD VERSION

Base:
- V26 calculator pages

Integrated:
- Final custom index.html
- Title: Cost and carbon comparator: Classic freezer Vs MVE Fusion
- Subtitle: Clic on the area you want a simulation
- Bottom helper text removed
- Middle East page included as middle-east.html

Upload all files and folders at the root of the GitHub Pages repository.

SEPARATED REGIONS UPDATE:
- asia.html and middle-east.html are now distinct files.
- asia.html excludes Middle East countries where present.
- middle-east.html contains only Middle East countries.
- Middle East countries loaded: 5

PDF REPORT UPDATE:
- Removed 'Download V10 Excel'
- Added 'Download PDF Report'
- Browser-generated printable PDF report with:
  * country
  * costs
  * CO2
  * annual savings
