# Get a Quote

> Ask for a quote to employ someone in Italy through ItalyTeams. Three short steps, answer within one business day.


<section> <span class="eyebrow">Get a quote</span> <h1 id="who-do-you-want-in-italy">Who do you want in Italy?</h1> <p class="lede">Three short steps. Within one business day you get the full cost and an outline of the contract.</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">Step 1 of 3</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">Work email<input id="email" type="email" autocomplete="email" required></label> <label for="company">Company<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">What do you need?<select id="service"><option>Employer of Record</option><option>Payroll (we have an Italian entity)</option><option>Work permit / Blue Card</option><option>Recruitment</option></select></label> <label for="count">How many people in Italy?<input id="count" type="number" min="1" value="1"></label> <label for="city">City or region<input id="city" placeholder="Milan, Turin, remote…"></label> <label for="salary">Planned annual gross salary (EUR)<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">Your name<input id="name" autocomplete="name"></label> <label for="start">Expected start date<input id="start" type="date"></label> <label for="notes">Anything else<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">I agree that ItalyTeams processes my data to answer this request (privacy policy).</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>Back</button><button type="submit" class="btn" id="next">Continue</button></div> </form> </section> <script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'italyteams'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `Step ${step} of 3`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? 'Send request' : 'Continue'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = 'Enter a valid work email to continue.'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = 'Tick the consent box so we can reply.'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = 'Thanks. You will hear from us within one business day.'; } });
</script>

<script>(function(){var sj=new URLSearchParams(location.search).get('subject');var n=document.getElementById('notes');if(sj&&n&&!n.value)n.value='['+sj.slice(0,40)+'] ';})();</script>


---
Source: https://italyteams.com/contact.html
