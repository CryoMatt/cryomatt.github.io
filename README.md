
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Conventional Tank vs Fusion Comparison</title>
  <style>
    :root { --bg:#f6f8fb; --card:#ffffff; --ink:#172033; --muted:#667085; --blue:#2563eb; --green:#059669; --orange:#ea580c; --line:#e5e7eb; }
    * { box-sizing: border-box; }
    body { margin:0; font-family: Arial, Helvetica, sans-serif; color:var(--ink); background:linear-gradient(180deg,#eef5ff,var(--bg)); }
    .wrap { max-width:1120px; margin:0 auto; padding:32px 18px; }
    header { margin-bottom:22px; }
    h1 { margin:0 0 8px; font-size:30px; }
    .sub { color:var(--muted); font-size:15px; }
    .grid { display:grid; grid-template-columns: 360px 1fr; gap:18px; align-items:start; }
    .card { background:var(--card); border:1px solid var(--line); border-radius:18px; padding:18px; box-shadow:0 8px 25px rgba(15,23,42,.06); }
    h2 { margin:0 0 14px; font-size:19px; }
    label { display:block; font-size:13px; color:var(--muted); margin:12px 0 5px; }
    input, select { width:100%; padding:10px 12px; border:1px solid #d0d5dd; border-radius:10px; font-size:15px; background:#fff; }
    input:focus, select:focus { outline:2px solid #bfdbfe; border-color:var(--blue); }
    .two { display:grid; grid-template-columns:1fr 1fr; gap:10px; }
    .kpis { display:grid; grid-template-columns: repeat(4, 1fr); gap:12px; margin-bottom:16px; }
    .kpi { border-radius:15px; padding:15px; border:1px solid var(--line); background:#fafafa; }
    .kpi b { display:block; font-size:22px; margin-top:8px; }
    .kpi span { color:var(--muted); font-size:13px; }
    .positive { color:var(--green); } .negative { color:#dc2626; }
    table { width:100%; border-collapse:collapse; overflow:hidden; border-radius:12px; }
    th, td { padding:12px 10px; border-bottom:1px solid var(--line); text-align:right; }
    th:first-child, td:first-child { text-align:left; }
    th { background:#f1f5f9; color:#344054; font-size:13px; }
    td { font-size:14px; }
    .classic { color:var(--blue); font-weight:700; }
    .fusion { color:var(--green); font-weight:700; }
    .bars { margin-top:18px; display:grid; gap:13px; }
    .bar-title { display:flex; justify-content:space-between; color:var(--muted); font-size:13px; margin-bottom:5px; }
    .bar-bg { background:#e5e7eb; border-radius:99px; overflow:hidden; height:14px; }
    .bar-fill { height:100%; border-radius:99px; transition:width .25s ease; }
    #priceClassicBar { background:var(--blue); } #priceFusionBar { background:var(--green); }
    #co2ClassicBar { background:var(--orange); } #co2FusionBar { background:var(--green); }
    .note { margin-top:14px; color:var(--muted); font-size:12px; line-height:1.45; }
    @media (max-width:900px){ .grid{grid-template-columns:1fr;} .kpis{grid-template-columns:1fr 1fr;} }
    @media (max-width:560px){ .kpis,.two{grid-template-columns:1fr;} h1{font-size:24px;} }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Cost & Carbon Footprint Comparison per Box</h1>
      <div class="sub">Comparison between a conventional liquid nitrogen tank and a Fusion system, with electricity prices by country based on the Excel table.</div>
    </header>

    <div class="grid">
      <section class="card">
        <h2>Input Data</h2>
        <label>Currency</label><select id="currency"><option value="EUR" selected>€ Euro</option><option value="USD">$ US Dollar</option></select>
        <label>Country for Electricity Price</label><select id="country"></select>
        <label id="ln2PriceLabel">LN₂ Price €/L</label><input id="ln2Price" type="number" step="0.01" value="1">
        <label>LN₂ Carbon Footprint kgCO₂/L</label><input id="ln2Co2" type="number" step="0.001" value="0.259" readonly>
        
        <label id="kwhPriceLabel">Electricity Price €/kWh</label><input id="kwhPrice" type="number" step="0.01" value="0.2" readonly>
        <p class="note" id="countryInfo" style="margin-top:6px"></p>
        <label>Electricity Carbon Footprint kgCO₂/kWh</label>
<input id="kwhCo2" type="number" step="0.001" value="0.020" readonly>
<p class="note" id="kwhCo2Info" style="margin-top:6px"></p>
        <div class="two">
          <div><label>Conventional Tank Boxes</label><input id="boxesClassic" type="number" step="1" value="364"></div>
          <div><label>Fusion Boxes</label><input id="boxesFusion" type="number" step="1" value="260"></div>
        </div>
        <label>Conventional Static Consumption L/day</label><input id="staticClassic" type="number" step="0.1" value="14">
        <label>Fusion Electricity Consumption kWh/year</label>
<input id="fusionKwhYear" type="number" step="1" value="8760">

<label>Fusion LN₂ Usage L/year</label>
<input id="fusionLn2Year" type="number" step="1" value="0">
        <p class="note">Formulas: conventional = L/day × 365 × LN₂ price or CO₂. Fusion = kWh/year × electricity price or CO₂. Per-box values are divided by the number of boxes for each solution.</p>
      </section>

      <main class="card">
        <h2>Annual Results</h2>
        <div class="kpis">
          <div class="kpi"><span>Cost Saving / Box</span><b id="savingPrice">-</b></div>
          <div class="kpi"><span>CO₂ Reduction / Box</span><b id="savingCo2">-</b></div>
          <div class="kpi"><span>Total Annual Savings</span><b id="savingTotalPrice">-</b></div>
          <div class="kpi"><span>Annual CO₂ Avoided</span><b id="savingTotalCo2">-</b></div>
        </div>

        <table>
          <thead><tr><th>Indicator</th><th>Conventional Tank</th><th>Fusion</th><th>Fusion Difference</th></tr></thead>
          <tbody>
            <tr><td>LN₂ Consumed / Year</td><td id="ln2Classic"></td><td id="ln2Fusion"></td><td id="ln2Diff"></td></tr>
            <tr><td>Electricity / Year</td><td id="kwhClassic"></td><td id="kwhFusion"></td><td id="kwhDiff"></td></tr>
            <tr><td>Annual Cost</td><td class="classic" id="priceClassic"></td><td class="fusion" id="priceFusion"></td><td id="priceDiff"></td></tr>
            <tr><td>Cost per Box</td><td class="classic" id="priceBoxClassic"></td><td class="fusion" id="priceBoxFusion"></td><td id="priceBoxDiff"></td></tr>
            <tr><td>Annual Carbon Footprint</td><td class="classic" id="co2Classic"></td><td class="fusion" id="co2Fusion"></td><td id="co2Diff"></td></tr>
            <tr><td>Carbon Footprint per Box</td><td class="classic" id="co2BoxClassic"></td><td class="fusion" id="co2BoxFusion"></td><td id="co2BoxDiff"></td></tr>
          </tbody>
        </table>

        <div class="bars">
          <div><div class="bar-title"><span>Cost per Box</span><span id="priceMax"></span></div><div class="bar-bg"><div id="priceClassicBar" class="bar-fill"></div></div><div class="bar-bg" style="margin-top:5px"><div id="priceFusionBar" class="bar-fill"></div></div></div>
          <div><div class="bar-title"><span>CO₂ per Box</span><span id="co2Max"></span></div><div class="bar-bg"><div id="co2ClassicBar" class="bar-fill"></div></div><div class="bar-bg" style="margin-top:5px"><div id="co2FusionBar" class="bar-fill"></div></div></div>
        </div>
      </main>
    </div>
  </div>

<script>
const COUNTRY_PRICES = [
  {country:'China', eur:0.07, usd:0.08, ln2co2:0.400, kwhco2:0.650, co2source:'IEA / Ember - https://ember-energy.org', note:'Highly regulated pricing', source:'GlobalPetrolPrices / estimations marché 2025'},
  {country:'United States', eur:0.16, usd:0.17, ln2co2:0.300, kwhco2:0.400, co2source:'IEA / EIA - https://www.iea.org', note:'Significant variation by state', source:'EIA / GlobalPetrolPrices'},
  {country:'Japan', eur:0.20, usd:0.22, ln2co2:0.320, kwhco2:0.500, co2source:'IEA / Ember - https://ember-energy.org', note:'High energy dependency', source:'METI / GlobalPetrolPrices'},
  {country:'France', eur:0.20, usd:0.22, ln2co2:0.259, kwhco2:0.020, co2source:'RTE (2025) - https://analysesetdonnees.rte-france.com', note:'Nuclear-dominated mix', source:'Eurostat / CRE'},
  {country:'Portugal', eur:0.22, usd:0.24, ln2co2:0.240, kwhco2:0.140, co2source:'EEA / Ember - https://ember-energy.org', note:'High taxes and grid costs', source:'Eurostat'},
  {country:'Spain', eur:0.24, usd:0.26, ln2co2:0.230, kwhco2:0.180, co2source:'EEA / Ember - https://www.eea.europa.eu', note:'Spot market influence', source:'Eurostat'},
  {country:'Germany', eur:0.39, usd:0.42, ln2co2:0.330, kwhco2:0.390, co2source:'Electricity Maps - https://www.electricitymaps.com', note:'Among the highest electricity prices', source:'Eurostat / BDEW'}
];
function val(id){ return parseFloat(document.getElementById(id).value) || 0; }
function currency(){ return document.getElementById('currency').value || 'EUR'; }
function symbol(){ return currency()==='USD' ? '$' : '€'; }
function selectedCountry(){ return COUNTRY_PRICES.find(c => c.country === document.getElementById('country').value) || COUNTRY_PRICES.find(c => c.country === 'France') || COUNTRY_PRICES[0]; }
function updateCountryPrice(){
  const c = selectedCountry();
  const price = currency()==='USD' ? c.usd : c.eur;
  document.getElementById('kwhPrice').value = price.toFixed(2);
  document.getElementById('ln2Co2').value = c.ln2co2.toFixed(3);
  document.getElementById('kwhCo2').value = c.kwhco2.toFixed(3);
  document.getElementById('countryInfo').textContent =
    c.country + ' : electricity price based on Excel table — ' + c.note +
    ' — Price source : ' + c.source;

  document.getElementById('kwhCo2Info').textContent =
    'Electricity CO₂ source : ' + c.co2source +
    ' | Value : ' + c.kwhco2.toFixed(3) + ' kgCO₂/kWh';
}
function money(n){ return n.toLocaleString('fr-FR',{style:'currency',currency:currency(),maximumFractionDigits:2}); }
function num(n,u='',d=2){ return n.toLocaleString('fr-FR',{maximumFractionDigits:d,minimumFractionDigits:d}) + u; }
function pct(part,max){ return max>0 ? Math.max(2, Math.min(100, part/max*100)) + '%' : '0%'; }
function set(id, text, cls=''){ const e=document.getElementById(id); e.textContent=text; e.className=cls; }
function diffText(v, unit, isMoney=false){ const cls = v<=0 ? 'positive' : 'negative'; const sign = v>0 ? '+' : ''; return {txt: sign + (isMoney ? money(v) : num(v,unit)), cls}; }
function calc(){
  updateCountryPrice();
  document.getElementById('ln2PriceLabel').textContent = 'LN₂ Price ' + symbol() + '/L';
  document.getElementById('kwhPriceLabel').textContent = 'Electricity Price ' + symbol() + '/kWh';
  const ln2Price=val('ln2Price'), ln2Co2=val('ln2Co2'), kwhPrice=val('kwhPrice'), kwhCo2=val('kwhCo2');
  const boxesClassic=Math.max(1,val('boxesClassic')), boxesFusion=Math.max(1,val('boxesFusion'));
  const staticClassic=val('staticClassic'), fusionKwhYear=val('fusionKwhYear'), fusionLn2Year=val('fusionLn2Year');
  const ln2Classic=staticClassic*365, ln2Fusion=fusionLn2Year;
  const kwhClassic=0, kwhFusion=fusionKwhYear;
  const priceClassic=ln2Classic*ln2Price, priceFusion=(kwhFusion*kwhPrice) + (ln2Fusion*ln2Price);
  const co2Classic=ln2Classic*ln2Co2, co2Fusion=(kwhFusion*kwhCo2) + (ln2Fusion*ln2Co2);
  const priceBoxClassic=priceClassic/boxesClassic, priceBoxFusion=priceFusion/boxesFusion;
  const co2BoxClassic=co2Classic/boxesClassic, co2BoxFusion=co2Fusion/boxesFusion;
  set('ln2Classic', num(ln2Classic,' L',0)); set('ln2Fusion', num(ln2Fusion,' L',0)); set('ln2Diff', num(ln2Fusion-ln2Classic,' L',0),'positive');
  set('kwhClassic', num(kwhClassic,' kWh',0)); set('kwhFusion', num(kwhFusion,' kWh',0)); set('kwhDiff','+'+num(kwhFusion-kwhClassic,' kWh',0),'negative');
  set('priceClassic', money(priceClassic)); set('priceFusion', money(priceFusion)); let d=diffText(priceFusion-priceClassic,'',true); set('priceDiff', d.txt, d.cls);
  set('priceBoxClassic', money(priceBoxClassic)); set('priceBoxFusion', money(priceBoxFusion)); d=diffText(priceBoxFusion-priceBoxClassic,'',true); set('priceBoxDiff', d.txt, d.cls);
  set('co2Classic', num(co2Classic,' kgCO₂')); set('co2Fusion', num(co2Fusion,' kgCO₂')); d=diffText(co2Fusion-co2Classic,' kgCO₂'); set('co2Diff', d.txt, d.cls);
  set('co2BoxClassic', num(co2BoxClassic,' kgCO₂')); set('co2BoxFusion', num(co2BoxFusion,' kgCO₂')); d=diffText(co2BoxFusion-co2BoxClassic,' kgCO₂'); set('co2BoxDiff', d.txt, d.cls);
  set('savingPrice', money(priceBoxClassic-priceBoxFusion), priceBoxClassic>=priceBoxFusion?'positive':'negative');
  set('savingCo2', num(co2BoxClassic-co2BoxFusion,' kgCO₂'), co2BoxClassic>=co2BoxFusion?'positive':'negative');
  set('savingTotalPrice', money(priceClassic-priceFusion), priceClassic>=priceFusion?'positive':'negative');
  set('savingTotalCo2', num(co2Classic-co2Fusion,' kgCO₂'), co2Classic>=co2Fusion?'positive':'negative');
  const pMax=Math.max(priceBoxClassic,priceBoxFusion); document.getElementById('priceMax').textContent='max '+money(pMax);
  document.getElementById('priceClassicBar').style.width=pct(priceBoxClassic,pMax); document.getElementById('priceFusionBar').style.width=pct(priceBoxFusion,pMax);
  const cMax=Math.max(co2BoxClassic,co2BoxFusion); document.getElementById('co2Max').textContent='max '+num(cMax,' kgCO₂');
  document.getElementById('co2ClassicBar').style.width=pct(co2BoxClassic,cMax); document.getElementById('co2FusionBar').style.width=pct(co2BoxFusion,cMax);
}
const countrySelect = document.getElementById('country');
COUNTRY_PRICES.forEach(c => {
  const opt = document.createElement('option');
  opt.value = c.country;
  opt.textContent = c.country;
  if (c.country === 'France') opt.selected = true;
  countrySelect.appendChild(opt);
});
document.querySelectorAll('input').forEach(i => i.addEventListener('input', calc));
document.querySelectorAll('select').forEach(s => s.addEventListener('change', calc));
calc();
</script>
</body>
</html>
