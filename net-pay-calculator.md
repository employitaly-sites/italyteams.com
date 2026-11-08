# Italy Net Pay Calculator

> Take-home pay in Italy in 2026 for Nordic employers making an offer: employee contributions, IRPEF with employment credits, regional and municipal surcharges in Milan, Rome, Turin, Bologna and Naples.


<section> <span class="eyebrow">Net pay calculator</span> <h1 id="take-home-pay-in-italy">Take-home pay in Italy</h1> <p class="lede">Italian candidates compare offers on net monthly pay. Enter the annual gross and the city to see it under 2026 rules: employee contributions, income tax with employment credits, local surcharges.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Annual gross salary, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">City<select id="city"><option>Milan</option><option>Rome</option><option>Turin</option><option>Bologna</option><option>Naples</option></select></label> <label for="inst">Paid in<select id="inst"><option value="13">13 instalments</option><option value="14">14 instalments</option><option value="12">12 (13th/14th spread monthly)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Item</th><th class="num">Per year</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="how-it-is-calculated">How it is calculated</h2> <ol> <li>Employee social security comes off first: 9.19% of gross, plus one extra point on pay above €56,224 a year.</li> <li>IRPEF is charged on the remainder, band by band: 23% to €28,000, 33% from €28,000 to €50,000, 43% beyond.</li> <li>Employment tax credits then cut the tax, most on low salaries, fading to nothing at €50,000; between €20,000 and €40,000 a further credit of up to €1,000 applies.</li> <li>Last come the regional and municipal surcharges, unless income sits under the city's exemption threshold.</li> </ol> <p>Unlike Swedish municipal tax, these surcharges are small, but they differ by city. None of it changes your employer cost: see the <a href="calculator.html">cost calculator</a>.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardy","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lazio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piedmont","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"Emilia-Romagna","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Campania","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−€' : '€') + Math.round(Math.abs(n)).toLocaleString('en-GB');
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Gross salary', g], ['Employee social security', -inps], ['Income tax (IRPEF) after credits', -irpef], [`Regional surcharge (${c.region})`, -reg], ['Municipal surcharge', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Net pay</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `About ${eur(net / n)} net in each of ${n} monthly payslips (${(net / g * 100).toFixed(0)}% of gross).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://italyteams.com/net-pay-calculator.html
