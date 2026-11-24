# Orbital and transit geometry

This chapter builds the geometry in two steps. The first section places a planet on its orbit in time and orients that orbit relative to the observer and the stellar spin. The second section zooms in on the stellar disk to show how those quantities shape a transit light curve.

## Orbit and angles

A planet's position at a given time follows from Kepler's laws. Time enters through the **mean anomaly**, the angle a planet would sweep if it moved uniformly,

$$
M=\frac{2\pi}{P}\,(t-t_p),
$$

where $t_p$ is the time of periastron passage. **Kepler's equation** connects it to the **eccentric anomaly** $E$, the angle measured at the center of the ellipse to a point on its circumscribed (auxiliary) circle:

$$
M=E-e\sin E .
$$

There is no closed-form solution for $E$, so codes solve it iteratively, usually by Newton's method. The **true anomaly** $\nu$, the angle at the star between periastron and the planet, and the star–planet distance $r$ follow from $E$:

$$
\tan\frac{\nu}{2}=\sqrt{\frac{1+e}{1-e}}\,\tan\frac{E}{2},
\qquad
r=a(1-e\cos E)=\frac{a(1-e^2)}{1+e\cos\nu}.
$$

For a circular orbit all three anomalies are equal. For an eccentric orbit the planet sweeps equal areas in equal times, so it hurries through periastron and lingers near apoastron.

Three angles orient the orbit relative to the observer.

| Angle | Definition | What a transit light curve tells you |
| --- | --- | --- |
| Inclination $i$ | Angle between the orbit normal and the line of sight. $i=90^\circ$ is edge-on. | Measured through the impact parameter, given $a/R_\star$, $e$, and $\omega$. |
| Argument of periastron $\omega$ | Angle in the orbital plane from the ascending node to periastron. | Transit occurs at $\nu=90^\circ-\omega$ and occultation at $\nu=270^\circ-\omega$. $e\sin\omega$ changes the transit duration. $e\cos\omega$ shifts the occultation away from phase 0.5 by roughly $(2P/\pi)\,e\cos\omega$. |
| Longitude of the ascending node $\Omega$ | Position angle on the sky, measured from north through east, of the ascending node. | Nothing. Rotating the whole system on the sky leaves the light curve unchanged. Astrometry, direct imaging, or mutual inclinations in multi-planet systems need $\Omega$. |

These definitions follow Winn (2010), as do the transit formulas in this book. The observer lies along $+Z$, so the ascending node is where the planet crosses the sky plane moving toward the observer, and $\omega$ describes the planet's orbit. Radial-velocity work usually points $+Z$ away from the observer and quotes the star's $\omega_\star=\omega+180^\circ$. The two changes cancel, so a published $\omega_\star$ can be used directly in these transit formulas. Check the convention of any code before passing it an angle.

The star's spin axis adds three more angles. The **projected obliquity** $\lambda$ is the sky-plane angle between the projected orbit normal and the projected stellar spin axis. Papers differ on its sign convention, and the figure below draws positive $\lambda$ clockwise. The true obliquity depends only on $\cos\lambda$. The **stellar inclination** $i_\star$ is the angle between the spin axis and the line of sight. Together with the orbital inclination they give the **true obliquity** $\psi$:

$$
\cos\psi=\cos i_\star\cos i+\sin i_\star\sin i\cos\lambda .
$$

A spherical star with radially symmetric limb darkening produces the same transit for any spin orientation, so these angles leave an ordinary light curve unchanged. The Rossiter–McLaughlin effect in spectroscopy measures $\lambda$ because the planet blocks approaching or receding parts of the rotating disk. Gravity darkening on rapid rotators and repeated spot crossings can constrain spin–orbit geometry from photometry. The stellar inclination follows from comparing $v\sin i_\star$ with the equatorial speed $v_{\rm eq}=2\pi R_\star/P_{\rm rot}$. Infer it with the correlation between $v_{\rm eq}$ and $v\sin i_\star$ taken into account rather than by dividing one by the other (Masuda & Winn 2020). For the formula for $\psi$ see Fabrycky & Winn (2009), and for a review see [Albrecht, Dawson & Winn (2022)](https://arxiv.org/abs/2203.05460).

```{raw} html
<style>
.tg {
  --tg-ink: var(--pst-color-text-base, #1f2328);
  --tg-muted: var(--pst-color-text-muted, #59636e);
  --tg-line: var(--pst-color-border, #d0d7de);
  --tg-panel: var(--pst-color-surface, #f6f8fa);
  --tg-accent: var(--pst-color-primary, #0a66c2);
  --tg-planet: #2f7fd8;
  --tg-warn: #b54708;
  font-size: 0.92rem;
  color: var(--tg-ink);
  margin: 1.5rem 0;
}
.tg-figs { display: grid; grid-template-columns: 1fr 1fr; grid-auto-flow: row dense; gap: 0.75rem; }
.tg-fig { margin: 0; background: var(--tg-panel); border: 1px solid var(--tg-line); border-radius: 6px; padding: 0.5rem; }
.tg-fig.tg-wide { grid-column: 1 / -1; }
.tg-fig figcaption { font-size: 0.8rem; color: var(--tg-muted); margin: 0 0 0.25rem; display: flex; justify-content: space-between; gap: 0.5rem; }
.tg-fig figcaption span:first-child { display: inline-flex; align-items: center; gap: 0.25rem; }
.tg-fig { transition: box-shadow 150ms ease, opacity 150ms ease; }
.tg-fig.tg-dragging { opacity: 0.85; box-shadow: 0 6px 18px rgba(0, 0, 0, 0.18); border-color: var(--tg-accent); }
.tg-fig canvas { display: block; width: 100%; }
.tg-c-sq { aspect-ratio: 1 / 1; }
.tg-c-lc { aspect-ratio: 2.6 / 1; }
.tg-controls { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 0.75rem; margin-top: 1rem; }
.tg-group { border: 1px solid var(--tg-line); border-radius: 6px; padding: 0.4rem 0.75rem 0.6rem; margin: 0; min-width: 0; }
.tg-group legend { font-size: 0.8rem; font-weight: 600; padding: 0 0.3rem; display: flex; align-items: center; gap: 0.3rem; }
.tg-handle { font: inherit; font-size: 0.95rem; line-height: 1; padding: 0.1rem 0.2rem; border: 0; border-radius: 3px; background: transparent; color: var(--tg-muted); cursor: grab; touch-action: none; }
.tg-handle:hover { color: var(--tg-accent); background: color-mix(in srgb, var(--tg-accent) 12%, transparent); }
.tg-handle:active { cursor: grabbing; }
.tg-handle:focus-visible { outline: 2px solid var(--tg-accent); outline-offset: 1px; }
.tg-group { transition: box-shadow 150ms ease, opacity 150ms ease; }
.tg-group.tg-dragging { opacity: 0.85; box-shadow: 0 6px 18px rgba(0, 0, 0, 0.18); border-color: var(--tg-accent); }
.tg-data-bar { margin: 0 0 0.35rem; gap: 0.3rem 0.8rem; }
.tg-data-bar label { font-size: 0.78rem; }
.tg-focus .tg-nontransit { display: none; }
.tg-hint { font-size: 0.72rem; color: var(--tg-muted); margin: 0 0 0.3rem; line-height: 1.3; }
.tg-ctl { display: grid; grid-template-columns: 1fr auto auto; align-items: center; gap: 0 0.5rem; margin-top: 0.25rem; }
.tg-ctl > label:first-child { font-size: 0.82rem; }
.tg-ctl output { font-variant-numeric: tabular-nums; font-size: 0.82rem; color: var(--tg-muted); }
.tg-hold { font-size: 0.72rem; color: var(--tg-muted); display: inline-flex; align-items: center; gap: 0.2rem; cursor: pointer; }
.tg-hold input { accent-color: var(--tg-accent); margin: 0; }
.tg-ctl input[type=range] { grid-column: 1 / -1; width: 100%; accent-color: var(--tg-accent); }
.tg-ctl input[type=range]:focus-visible, .tg-hold input:focus-visible { outline: 2px solid var(--tg-accent); outline-offset: 2px; }
.tg-bar { display: flex; flex-wrap: wrap; gap: 0.5rem 1rem; align-items: center; margin-top: 1rem; }
.tg-bar select, .tg-bar button { font: inherit; font-size: 0.85rem; padding: 0.25rem 0.6rem; border: 1px solid var(--tg-line); border-radius: 4px; background: transparent; color: var(--tg-ink); cursor: pointer; }
.tg-bar button:hover, .tg-bar select:hover { border-color: var(--tg-accent); }
.tg-bar button:focus-visible, .tg-bar select:focus-visible { outline: 2px solid var(--tg-accent); outline-offset: 1px; }
.tg-bar label { font-size: 0.85rem; display: inline-flex; gap: 0.35rem; align-items: center; }
.tg-out { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 0.5rem 1rem; margin: 1rem 0 0; padding: 0.75rem; border-top: 1px solid var(--tg-line); }
.tg-out div { display: flex; flex-direction: column; }
.tg-out dt { font-size: 0.75rem; color: var(--tg-muted); }
.tg-out dd { margin: 0; font-variant-numeric: tabular-nums; font-size: 1rem; }
.tg-status { margin-top: 0.5rem; font-size: 0.85rem; color: var(--tg-warn); min-height: 1.2em; }
@media (max-width: 640px) { .tg-figs { grid-template-columns: 1fr; } }
</style>
<div class="tg" id="ob-root">
  <div class="tg-figs">
    <figure class="tg-fig">
      <figcaption><span>Orbital plane, seen from above</span><span>observer ↓</span></figcaption>
      <canvas class="tg-c-sq" id="ob-plane" role="img" aria-label="Orbital plane with mean, eccentric, and true anomalies"></canvas>
    </figure>
    <figure class="tg-fig">
      <figcaption><span>Orbit projected on the sky</span><span>N up, E left · schematic</span></figcaption>
      <canvas class="tg-c-sq" id="ob-sky" role="img" aria-label="Sky-plane projection of the orbit with the ascending node"></canvas>
    </figure>
    <figure class="tg-fig">
      <figcaption><span>Stellar spin and transit chord</span><span>projected orbit normal ↑</span></figcaption>
      <canvas class="tg-c-sq" id="ob-spin" role="img" aria-label="Stellar disk with spin axis, equator, and transit chord"></canvas>
    </figure>
    <figure class="tg-fig">
      <figcaption><span>Values at this instant</span><span></span></figcaption>
      <dl class="tg-out ob-out" id="ob-out" aria-live="polite"></dl>
    </figure>
  </div>
  <div class="tg-bar">
    <label>Preset
      <select id="ob-preset">
        <option>Default</option>
        <option>Circular, aligned</option>
        <option>Eccentric, transit at periastron</option>
        <option>Eccentric, transit at apoastron</option>
        <option>Highly eccentric (e = 0.9)</option>
        <option>Misaligned (λ = 60°)</option>
        <option>Polar orbit</option>
        <option>Retrograde orbit</option>
        <option>Pole-on star</option>
        <option>Face-on orbit (no transit)</option>
      </select>
    </label>
    <button type="button" id="ob-reset">Reset</button>
    <button type="button" id="ob-layout">Reset layout</button>
    <label><input type="checkbox" id="ob-focus"> Only parameters that shape the transit</label>
  </div>
  <div class="tg-controls" id="ob-controls"></div>
  <p class="tg-status" id="ob-status" role="status"></p>
</div>

<script>
(function () {
  "use strict";
  var root = document.getElementById("ob-root");
  if (!root) return;

  // Shared: mark sliders that leave the transit light curve unchanged and let a checkbox hide them.
  window.transitBookFocus = window.transitBookFocus || function (root, container, keys, checkbox, onChange) {
    container.querySelectorAll(".tg-ctl").forEach(function (row) {
      var inp = row.querySelector('input[type="range"]');
      if (inp && keys[inp.id.replace(/^(ob|tg)-in-/, "")]) row.classList.add("tg-nontransit");
    });
    container.querySelectorAll(".tg-group").forEach(function (card) {
      var rows = card.querySelectorAll(".tg-ctl");
      if (rows.length && card.querySelectorAll(".tg-ctl.tg-nontransit").length === rows.length) card.classList.add("tg-nontransit");
    });
    checkbox.addEventListener("change", function () { root.classList.toggle("tg-focus", checkbox.checked); onChange(); });
  };

  // Mean dwarf sequence of Pecaut & Mamajek (2013), Mamajek's online table version 2022.04.16,
  // O3V–M9.5V: Teff (K) and R★ (R☉). Interpolated linearly in log Teff and log R.
  window.transitBookMainSequence = window.transitBookMainSequence || (function () {
    var rows = "44900 13.43|42900 12.13|41400 11.45|40500 10.71|39500 10.27|38300 9.82|37100 9.42|36100 8.95|35100 8.47|34300 8.06|33300 7.72|31900 7.50|31400 7.16|29000 6.48|26000 5.71|24500 5.02|20600 4.06|18500 3.89|17000 3.61|16400 3.46|15700 3.36|14500 3.27|14000 2.94|12300 2.86|10700 2.49|10400 2.45|9700 2.193|9300 2.136|8800 2.117|8600 1.861|8250 1.794|8100 1.785|7910 1.775|7760 1.750|7590 1.747|7400 1.747|7220 1.728|7020 1.679|6820 1.622|6750 1.578|6670 1.533|6550 1.473|6350 1.359|6280 1.324|6180 1.221|6050 1.167|5990 1.142|5930 1.100|5860 1.060|5770 1.012|5720 1.002|5680 0.991|5660 0.977|5600 0.949|5550 0.927|5480 0.914|5380 0.853|5270 0.813|5170 0.797|5100 0.783|4830 0.755|4600 0.713|4440 0.701|4300 0.669|4100 0.630|3990 0.615|3930 0.608|3850 0.588|3770 0.544|3660 0.501|3620 0.482|3560 0.446|3470 0.421|3430 0.361|3270 0.300|3210 0.274|3110 0.217|3060 0.196|2930 0.156|2810 0.137|2740 0.126|2680 0.120|2630 0.116|2570 0.114|2420 0.104|2380 0.102|2350 0.101".split("|").map(function (r) { var f = r.split(" "); return [Math.log10(+f[0]), Math.log10(+f[1])]; });
    function interp(x, pts) {
      if (x <= pts[0][0]) return pts[0][1];
      if (x >= pts[pts.length - 1][0]) return pts[pts.length - 1][1];
      var j = 1;
      while (pts[j][0] < x) j++;
      var dx = pts[j][0] - pts[j - 1][0];
      return dx === 0 ? pts[j][1] : pts[j - 1][1] + (x - pts[j - 1][0]) / dx * (pts[j][1] - pts[j - 1][1]);
    }
    var byR = rows.map(function (r) { return [r[1], r[0]]; }).sort(function (a, b) { return a[0] - b[0]; });
    var byT = rows.slice().sort(function (a, b) { return a[0] - b[0]; });
    return {
      teffFromR: function (R) { return Math.pow(10, interp(Math.log10(R), byR)); },
      rFromTeff: function (T) { return Math.pow(10, interp(Math.log10(T), byT)); }
    };
  })();

  // Harre & Heller (2021, Astron. Nachr., arXiv:2101.06254), Table 5: main-sequence spectral type, Teff (K),
  // and disk-center color (linear RGB stored as hex). Colors between rows are linearly interpolated.
  window.transitBookStarColor = window.transitBookStarColor || (function () {
    var TABLE = "M9.5V 2300 ff7d24|M9V 2400 ff842d|M8V 2500 ff8a33|M7.5V 2600 ff9a41|M6.5V 2700 ffa548|M6V 2800 ffa548|M6V 2900 ffa448|M5.5V 3000 ffa349|M4.5V 3100 ffa24a|M4V 3200 ffa24c|M3.5V 3300 ffa24e|M3V 3400 ffa250|M2.5V 3500 ffa251|M2V 3600 ffa153|M1V 3700 ffa256|M0.5V 3800 ffa158|M0V 3900 ffa25a|K8V 4000 ffa35e|K7V 4100 ffa563|K6.5V 4200 ffa868|K5.5V 4300 ffac6f|K5V 4400 ffb177|K4.5V 4500 ffb67f|K4V 4600 ffbc87|K3.5V 4700 ffc18f|K3V 4800 ffc797|K3V 4900 ffcc9f|K2.5V 5000 ffd1a7|K1.5V 5100 ffd6b0|K1V 5200 ffdab8|K0V 5300 ffdec0|G9V 5400 ffe1c7|G8V 5500 ffe5cf|G6V 5600 ffe8d7|G4V 5700 ffebdf|G2V 5800 ffede6|G1V 5900 ffefed|F9.5V 6000 fff2f6|F9V 6100 fff4fe|F8V 6200 f3edff|F6V 6300 ebe7ff|F6V 6400 e4e3ff|F5V 6500 dddeff|F4V 6600 d7d9ff|F3V 6700 d1d6ff|F2V 6800 ccd2ff|F2V 6900 c7cfff|F1V 7000 c2cbff|F0V 7200 b8c5ff|A9V 7400 b0bfff|A8V 7600 abbcff|A7V 7800 a2b5ff|A6V 8000 9ab0ff|A4V 8200 93aaff|A4V 8400 8da6ff|A3V 8600 8ba4ff|A2V 8800 87a1ff|A2V 9000 849fff|A1V 9200 819dff|A1V 9400 7f9bff|A0V 9600 7d99ff|A0V 9800 7b97ff|A0V 10000 7996ff|B9.5V 10200 7895ff|B9.5V 10400 7794ff|B9V 10600 7693ff|B9V 10800 7592ff|B9V 11000 7491ff|B9V 11200 7391ff|B9V 11400 7290ff|B9V 11600 7290ff|B8V 11800 728fff|B8V 12000 718fff|B8V 12500 708eff|B8V 13000 6f8dff|B7V 13500 6e8cff|B7V 14000 6d8bff|B6V 14500 6c8aff|B6V 15000 6b89ff|B5V 16000 6988ff|B3V 17000 6887ff|B2.5V 18000 6686ff|B2.5V 19000 6585ff|B2V 20000 6484ff|B2V 21000 6383ff|B2V 22000 6283ff|B1.5V 23000 6182ff|B1.5V 24000 6181ff|B1.5V 25000 6081ff|B1V 26000 5f80ff|B1V 27000 5f80ff|B1V 27500 5e7fff|B0.5V 28000 5e7fff|B0.5V 29000 5d7fff|B0.5V 30000 5d7eff|O9.5V 32500 5c7dff|O8V 35000 5b7cff|O6V 37500 5b7cff|O5V 40000 5b7bff|O4V 42500 5a7bff|O4V 45000 5a7bff|O3V 47500 5b7bff|O2V 50000 5b7bff|O2V 52500 5b7cff|O1V 55000 5c7cff".split("|").map(function (row) {
      var f = row.split(" "), h = f[2];
      return { spt: f[0], teff: +f[1], rgb: [parseInt(h.slice(0, 2), 16), parseInt(h.slice(2, 4), 16), parseInt(h.slice(4, 6), 16)], hex: "#" + h };
    });
    return function (teff) {
      var t = Math.max(TABLE[0].teff, Math.min(TABLE[TABLE.length - 1].teff, teff)), j = 1;
      while (j < TABLE.length - 1 && TABLE[j].teff < t) j++;
      var a = TABLE[j - 1], b = TABLE[j], w = (t - a.teff) / (b.teff - a.teff);
      var rgb = a.rgb.map(function (v, k) { return v + w * (b.rgb[k] - v); });
      var near = w < 0.5 ? a : b;
      var hex = "#" + rgb.map(function (v) { return Math.round(v).toString(16).padStart(2, "0"); }).join("");
      return { rgb: rgb, hex: hex, spt: near.spt };
    };
  })();

  // Limb-darkened disk in the star's color: intensity multiplies the disk-center color.
  window.transitBookStarGradient = window.transitBookStarGradient || function (ctx, cx, cy, R, teff, u1, u2) {
    var base = window.transitBookStarColor(teff).rgb, g = ctx.createRadialGradient(cx, cy, 0, cx, cy, R);
    for (var i = 0; i <= 12; i++) {
      var r = Math.min(i / 12, 0.999), mu = Math.sqrt(1 - r * r);
      var I = Math.max(0, 1 - u1 * (1 - mu) - u2 * (1 - mu) * (1 - mu));
      g.addColorStop(r, "rgb(" + base.map(function (v) { return Math.round(v * I); }).join(",") + ")");
    }
    return g;
  };

  // Shared by both figures: drag a card by its grip (pointer or touch), or focus the grip and use arrow keys.
  window.transitBookSortable = window.transitBookSortable || function (container, storageKey, resetButton, opts) {
    var item = (opts && opts.item) || ".tg-group", changed = (opts && opts.onChange) || function () {};
    var defaultOrder = Array.prototype.map.call(container.children, function (c) { return c.dataset.key; });
    function save() {
      changed();
      try { localStorage.setItem(storageKey, JSON.stringify(Array.prototype.map.call(container.children, function (c) { return c.dataset.key; }))); } catch (err) { /* storage unavailable */ }
    }
    function apply(order) {
      order.forEach(function (key) {
        var el = container.querySelector('[data-key="' + key + '"]');
        if (el) container.appendChild(el);
      });
    }
    try { var saved = JSON.parse(localStorage.getItem(storageKey) || "null"); if (Array.isArray(saved)) apply(saved); } catch (err) { /* ignore */ }

    container.addEventListener("pointerdown", function (e) {
      var handle = e.target.closest(".tg-handle");
      if (!handle || !container.contains(handle)) return;
      var card = handle.closest(item);
      e.preventDefault();
      card.classList.add("tg-dragging");
      // Listen on the document: moving the card in the DOM would drop pointer capture on the grip.
      function move(ev) {
        var el = document.elementFromPoint(ev.clientX, ev.clientY), over = el && el.closest(item);
        if (!over || over === card || over.parentNode !== container) return;
        var kids = Array.prototype.slice.call(container.children);
        container.insertBefore(card, kids.indexOf(over) > kids.indexOf(card) ? over.nextSibling : over);
      }
      function end(ev) {
        if (ev.type === "pointerup") move(ev);
        card.classList.remove("tg-dragging");
        document.removeEventListener("pointermove", move);
        document.removeEventListener("pointerup", end);
        document.removeEventListener("pointercancel", end);
        save();
      }
      document.addEventListener("pointermove", move);
      document.addEventListener("pointerup", end);
      document.addEventListener("pointercancel", end);
    });
    container.addEventListener("keydown", function (e) {
      var handle = e.target.closest(".tg-handle");
      if (!handle) return;
      var card = handle.closest(item), back = e.key === "ArrowUp" || e.key === "ArrowLeft", fwd = e.key === "ArrowDown" || e.key === "ArrowRight";
      if (!back && !fwd) return;
      e.preventDefault();
      if (back && card.previousElementSibling) container.insertBefore(card, card.previousElementSibling);
      if (fwd && card.nextElementSibling) container.insertBefore(card.nextElementSibling, card);
      handle.focus(); save();
    });
    if (resetButton) resetButton.addEventListener("click", function () { apply(defaultOrder); save(); });
  };

  // Give each plot panel a key and a grip in its caption so the panel grid can be reordered.
  window.transitBookFigureGrips = window.transitBookFigureGrips || function (figs) {
    Array.prototype.forEach.call(figs.children, function (fig, i) {
      var canvas = fig.querySelector("canvas, dl"), first = fig.querySelector("figcaption span");
      fig.dataset.key = canvas ? canvas.id : "fig" + i;
      var title = first.textContent, grip = document.createElement("button");
      grip.type = "button"; grip.className = "tg-handle"; grip.textContent = "⠿";
      grip.title = "Drag to move this panel";
      grip.setAttribute("aria-label", "Move the " + title + " panel. Drag, or use the arrow keys.");
      first.prepend(grip);
    });
  };

  window.transitBookCardTitle = window.transitBookCardTitle || function (legend, title) {
    var grip = document.createElement("button");
    grip.type = "button"; grip.className = "tg-handle"; grip.textContent = "⠿";
    grip.title = "Drag to move this card";
    grip.setAttribute("aria-label", "Move the " + title + " card. Drag, or use the arrow keys.");
    legend.append(grip, document.createTextNode(title));
  };
  var D = Math.PI / 180, TAU = 2 * Math.PI, RSUN_KM = 695700, DAY = 86400;
  var COL = { M: "#15803d", E: "#c2410c", nu: "#2f7fd8", w: "#7c3aed" };

  // name, label, min, max, step, decimals, group, default
  var SPECS = [
    ["t", "Time since periastron (fraction of P)", 0, 1, 0.001, 3, "shape", 0.12],
    ["e", "Eccentricity e", 0, 0.9, 0.001, 3, "shape", 0.5],
    ["w", "Argument of periastron ω (°)", -180, 180, 0.5, 1, "shape", 60],
    ["i", "Orbital inclination i (°)", 0, 180, 0.1, 1, "orient", 87],
    ["W", "Longitude of ascending node Ω (°)", 0, 360, 0.5, 1, "orient", 30],
    ["aRs", "a/R★ (star size in sky views)", 2, 30, 0.1, 1, "orient", 8],
    ["istar", "Stellar inclination i★ (°)", 0, 180, 0.5, 1, "spin", 70],
    ["lam", "Projected obliquity λ (°)", -180, 180, 0.5, 1, "spin", 25],
    ["prot", "Rotation period P_rot (days)", 0.3, 60, 0.1, 1, "spin", 10],
    ["rs", "Stellar radius R★ (R☉)", 0.1, 5, 0.01, 2, "spin", 1],
    ["teff", "Effective temperature T_eff (K)", 2300, 55000, 0.002, 0, "spin", 5800, true]
  ];
  var GROUPS = [["shape", "Orbit shape and timing"], ["orient", "Orbit orientation"], ["spin", "Stellar spin"]];
  var st = {}, inputs = {}, outputs = {}, PANEL = "#fff";

  var box = document.getElementById("ob-controls");
  GROUPS.forEach(function (g) {
    var fs = document.createElement("fieldset"), lg = document.createElement("legend");
    fs.className = "tg-group"; fs.dataset.key = g[0]; window.transitBookCardTitle(lg, g[1]); fs.appendChild(lg);
    SPECS.filter(function (s) { return s[6] === g[0]; }).forEach(function (s) {
      var id = "ob-in-" + s[0], wrap = document.createElement("div");
      wrap.className = "tg-ctl";
      var lab = document.createElement("label"); lab.htmlFor = id; lab.textContent = s[1];
      var out = document.createElement("output"); out.htmlFor = id;
      var inp = document.createElement("input");
      var lg10 = !!s[8];
      inp.type = "range"; inp.id = id;
      inp.min = lg10 ? Math.log10(s[2]) : s[2]; inp.max = lg10 ? Math.log10(s[3]) : s[3];
      inp.step = s[4]; inp.value = lg10 ? Math.log10(s[7]) : s[7];
      inp.addEventListener("input", function () { var v = parseFloat(inp.value); st[s[0]] = lg10 ? Math.pow(10, v) : v; linkMainSequence(s[0]); draw(); });
      wrap.append(lab, out, document.createElement("span"), inp);
      fs.appendChild(wrap);
      st[s[0]] = s[7]; inputs[s[0]] = inp; outputs[s[0]] = out;
    });
    box.appendChild(fs);
  });

  // With the main-sequence option on, R★ and T_eff move together along the dwarf sequence.
  var msOn = true, MS = window.transitBookMainSequence;
  function linkMainSequence(name) {
    if (!msOn) return;
    if (name === "rs") { st.teff = MS.teffFromR(st.rs); inputs.teff.value = Math.log10(st.teff); }
    if (name === "teff") { st.rs = MS.rFromTeff(st.teff); inputs.rs.value = st.rs; }
  }
  (function addMainSequenceToggle() {
    var grp = box.querySelector('[data-key="spin"]'), row = document.createElement("div");
    row.className = "tg-bar tg-data-bar";
    var lab = document.createElement("label"), cb = document.createElement("input");
    cb.type = "checkbox"; cb.checked = msOn;
    cb.addEventListener("change", function () { msOn = cb.checked; linkMainSequence("rs"); draw(); });
    lab.append(cb, document.createTextNode(" Main-sequence star: link T_eff and R★"));
    row.appendChild(lab); grp.insertBefore(row, grp.children[1]);
  })();

  function solveKepler(M, e) {
    var E = e < 0.8 ? M : Math.PI;
    for (var k = 0; k < 50; k++) {
      var d = (E - e * Math.sin(E) - M) / (1 - e * Math.cos(E));
      E -= d;
      if (Math.abs(d) < 1e-12) break;
    }
    return E;
  }
  function wrap2pi(x) { return ((x % TAU) + TAU) % TAU; }

  // Presets override the defaults. atTransit places the planet at mid-transit (ν = 90° − ω).
  var BASE = {};
  SPECS.forEach(function (s) { BASE[s[0]] = s[7]; });
  var PRESETS = {
    "Default": {},
    "Circular, aligned": { e: 0, w: 90, i: 89, istar: 90, lam: 0, atTransit: true },
    "Eccentric, transit at periastron": { e: 0.5, w: 90, i: 89, atTransit: true },
    "Eccentric, transit at apoastron": { e: 0.5, w: -90, i: 89, atTransit: true },
    "Highly eccentric (e = 0.9)": { e: 0.9, w: -60, i: 89.3, aRs: 25, atTransit: true },
    "Misaligned (λ = 60°)": { e: 0, w: 90, i: 88, istar: 60, lam: 60, atTransit: true },
    "Polar orbit": { e: 0, w: 90, i: 88, istar: 90, lam: 90, atTransit: true },
    "Retrograde orbit": { e: 0, w: 90, i: 88, istar: 90, lam: 180, atTransit: true },
    "Pole-on star": { e: 0, w: 90, i: 88, istar: 0, lam: 30, atTransit: true },
    "Face-on orbit (no transit)": { e: 0.3, w: 45, i: 0, W: 30 }
  };
  function transitTime(e, wDeg) {
    var nu = Math.PI / 2 - wDeg * D;
    var E = 2 * Math.atan(Math.sqrt((1 - e) / (1 + e)) * Math.tan(nu / 2));
    return wrap2pi(E - e * Math.sin(E)) / TAU;
  }
  function loadPreset(name) {
    var p = PRESETS[name];
    SPECS.forEach(function (s) {
      var v = p[s[0]] != null ? p[s[0]] : BASE[s[0]];
      st[s[0]] = v; inputs[s[0]].value = s[8] ? Math.log10(v) : v;
    });
    if (p.atTransit) { st.t = transitTime(st.e, st.w); inputs.t.value = st.t; }
    linkMainSequence("rs");
    draw();
  }
  document.getElementById("ob-preset").addEventListener("change", function (ev) { loadPreset(ev.target.value); });
  document.getElementById("ob-reset").addEventListener("click", function () {
    document.getElementById("ob-preset").value = "Default"; loadPreset("Default");
  });

  function colors() {
    var cs = getComputedStyle(root), g = function (n) { return cs.getPropertyValue(n).trim(); };
    PANEL = getComputedStyle(root.querySelector(".tg-fig")).backgroundColor;
    return { ink: g("--tg-ink"), muted: g("--tg-muted"), line: g("--tg-line"), accent: g("--tg-accent"), planet: g("--tg-planet") };
  }
  function prep(id) {
    var c = document.getElementById(id), dpr = window.devicePixelRatio || 1, w = c.clientWidth, h = c.clientHeight;
    if (c.width !== Math.round(w * dpr) || c.height !== Math.round(h * dpr)) { c.width = Math.round(w * dpr); c.height = Math.round(h * dpr); }
    var ctx = c.getContext("2d");
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0); ctx.clearRect(0, 0, w, h);
    ctx.font = "12px sans-serif"; ctx.lineCap = "round";
    return { ctx: ctx, w: w, h: h };
  }
  function dot(ctx, p, r, fill, hollow) {
    ctx.beginPath(); ctx.arc(p[0], p[1], r, 0, TAU);
    if (hollow) { ctx.strokeStyle = fill; ctx.lineWidth = 1.5; ctx.stroke(); } else { ctx.fillStyle = fill; ctx.fill(); }
  }
  function line(ctx, a, b, color, width, dash) {
    ctx.strokeStyle = color; ctx.lineWidth = width || 1; ctx.setLineDash(dash || []);
    ctx.beginPath(); ctx.moveTo(a[0], a[1]); ctx.lineTo(b[0], b[1]); ctx.stroke(); ctx.setLineDash([]);
  }
  function poly(ctx, pts, color, width, dash) {
    if (pts.length < 2) return;
    ctx.strokeStyle = color; ctx.lineWidth = width || 1; ctx.setLineDash(dash || []);
    ctx.beginPath(); ctx.moveTo(pts[0][0], pts[0][1]);
    for (var k = 1; k < pts.length; k++) ctx.lineTo(pts[k][0], pts[k][1]);
    ctx.stroke(); ctx.setLineDash([]);
  }
  function label(ctx, text, p, color, align, bold, halo) {
    ctx.save();
    ctx.font = (bold ? "600 14px" : "12px") + " sans-serif";
    ctx.textAlign = align || "left"; ctx.textBaseline = bold ? "middle" : "alphabetic";
    ctx.lineJoin = "round"; ctx.lineWidth = 4; ctx.strokeStyle = halo || PANEL;
    ctx.strokeText(text, p[0], p[1]); ctx.fillStyle = color; ctx.fillText(text, p[0], p[1]);
    ctx.restore();
  }
  // Illustrative quadratic limb darkening for the color figure.
  function star(ctx, p, r) {
    ctx.beginPath(); ctx.arc(p[0], p[1], r, 0, TAU);
    ctx.fillStyle = window.transitBookStarGradient(ctx, p[0], p[1], r, st.teff, 0.45, 0.2); ctx.fill();
    ctx.strokeStyle = "rgba(0,0,0,0.25)"; ctx.lineWidth = 1; ctx.stroke();
  }

  // Orbit state shared by all panels. Angles in radians, distances in units of a.
  function orbit() {
    var e = st.e, w = st.w * D, M = TAU * st.t;
    var E = wrap2pi(solveKepler(M, e));
    var nu = wrap2pi(2 * Math.atan2(Math.sqrt(1 + e) * Math.sin(E / 2), Math.sqrt(1 - e) * Math.cos(E / 2)));
    return { e: e, w: w, M: M, E: E, nu: nu, r: 1 - e * Math.cos(E) };
  }

  // Orbital plane: angle th from the ascending node; canvas x = -r cos th, y = +r sin th (observer down).
  function drawPlane(o, C) {
    var d = prep("ob-plane"), ctx = d.ctx, e = o.e, w = o.w;
    var s = Math.min(d.w, d.h) / (2 * 1.16);
    var P = function (th, rr, org) { return [org[0] - rr * Math.cos(th) * s, org[1] + rr * Math.sin(th) * s]; };
    var sc = P(w, e, [d.w / 2, d.h / 2]);
    var arc = function (org, rr, th0, dth, n) {
      var pts = []; n = n || 40;
      for (var k = 0; k <= n; k++) pts.push(P(th0 + dth * k / n, rr, org));
      return pts;
    };
    var cen = P(w + Math.PI, e, sc);

    line(ctx, [8, sc[1]], [d.w - 8, sc[1]], C.line, 1, [4, 4]);
    label(ctx, "☊", [12, sc[1] - 6], C.muted); label(ctx, "☋", [d.w - 12, sc[1] - 6], C.muted, "right");
    line(ctx, sc, [sc[0], d.h - 4], C.line, 1, [2, 4]);

    ctx.strokeStyle = C.line; ctx.setLineDash([3, 3]); ctx.beginPath(); ctx.arc(cen[0], cen[1], s, 0, TAU); ctx.stroke(); ctx.setLineDash([]);
    var ell = [];
    for (var k = 0; k <= 240; k++) { var v = TAU * k / 240; ell.push(P(w + v, (1 - e * e) / (1 + e * Math.cos(v)), sc)); }
    poly(ctx, ell, C.muted, 1.5);
    line(ctx, P(w, 1 - e, sc), P(w + Math.PI, 1 + e, sc), C.line, 1);

    [[Math.PI / 2, "transit"], [1.5 * Math.PI, "occultation"]].forEach(function (m) {
      var v = m[0] - w, p = P(m[0], (1 - e * e) / (1 + e * Math.cos(v)), sc);
      dot(ctx, p, 3, C.muted); label(ctx, m[1], [p[0] + 6, p[1] + (m[0] > Math.PI ? -6 : 14)], C.muted);
    });
    if (e > 0.005) {
      var peri = P(w, 1 - e, sc), rw = 0.25 + 0.4 * (1 - e);
      dot(ctx, peri, 3.5, COL.w); label(ctx, "periastron", [peri[0] + 6, peri[1] - 6], COL.w);
      poly(ctx, arc(sc, rw, 0, w), COL.w, 2);
      label(ctx, "ω", P(w / 2, rw + 0.1, sc), COL.w, "center", true);
    }
    poly(ctx, arc(sc, 0.16, w, o.nu), COL.nu, 2.5);
    label(ctx, "ν", P(w + o.nu / 2, 0.26, sc), COL.nu, "center", true);
    if (e > 0.005) {
      poly(ctx, arc(cen, 0.3, w, o.M), COL.M, 2.5);
      label(ctx, "M", P(w + o.M / 2, 0.39, cen), COL.M, "center", true);
      poly(ctx, arc(cen, 0.46, w, o.E), COL.E, 2.5);
      label(ctx, "E", P(w + o.E / 2, 0.55, cen), COL.E, "center", true);
      dot(ctx, cen, 2.5, C.muted);
    }

    var mPt = P(w + o.M, 1, cen), ePt = P(w + o.E, 1, cen), pl = P(w + o.nu, o.r, sc);
    line(ctx, cen, mPt, COL.M, 1, [3, 3]); dot(ctx, mPt, 4, COL.M, true);
    line(ctx, cen, ePt, COL.E, 1); dot(ctx, ePt, 3, COL.E);
    line(ctx, ePt, pl, C.muted, 1, [1, 3]);
    line(ctx, sc, pl, COL.nu, 1);
    star(ctx, sc, 6); dot(ctx, pl, 5, C.planet);
  }

  // Sky plane: node direction at position angle Ω (N through E), N up and E left.
  function drawSky(o, C) {
    var d = prep("ob-sky"), ctx = d.ctx, e = o.e, w = o.w, inc = st.i * D, W = st.W * D;
    var ext = (1 + e) * 1.2, s = Math.min(d.w, d.h) / (2 * ext), c = [d.w / 2, d.h / 2];
    var n = [-Math.sin(W), -Math.cos(W)], q = [-Math.cos(W), Math.sin(W)];
    var sky = function (th, rr) {
      var X = rr * Math.cos(th), Y = rr * Math.sin(th) * Math.cos(inc);
      return { p: [c[0] + s * (X * n[0] + Y * q[0]), c[1] + s * (X * n[1] + Y * q[1])], z: rr * Math.sin(th) * Math.sin(inc) };
    };
    var back = [], front = [], k;
    for (k = 0; k <= 240; k++) {
      var v = TAU * k / 240, pt = sky(w + v, (1 - e * e) / (1 + e * Math.cos(v)));
      (pt.z < 0 ? back : front).push(pt.p);
      if (pt.z < 0 && front.length) { poly(ctx, front, C.muted, 1.5); front = []; }
      if (pt.z >= 0 && back.length) { poly(ctx, back, C.muted, 1, [2, 4]); back = []; }
    }
    poly(ctx, front, C.muted, 1.5); poly(ctx, back, C.muted, 1, [2, 4]);

    line(ctx, [c[0] - n[0] * ext * s, c[1] - n[1] * ext * s], [c[0] + n[0] * ext * s, c[1] + n[1] * ext * s], C.line, 1, [4, 4]);
    var asc = [c[0] + n[0] * (ext - 0.12) * s, c[1] + n[1] * (ext - 0.12) * s];
    label(ctx, "☊", [asc[0] + 4, asc[1] - 4], C.muted);
    line(ctx, c, [c[0], c[1] - 0.55 * ext * s], C.line, 1);
    var pts = [];
    for (k = 0; k <= 40; k++) { var a = W * k / 40; pts.push([c[0] - 0.35 * ext * s * Math.sin(a), c[1] - 0.35 * ext * s * Math.cos(a)]); }
    poly(ctx, pts, C.accent, 2);
    var mid = [c[0] - 0.43 * ext * s * Math.sin(W / 2), c[1] - 0.43 * ext * s * Math.cos(W / 2)];
    label(ctx, "Ω", mid, C.accent, "center", true);

    line(ctx, [24, 40], [24, 14], C.ink, 1.5); label(ctx, "N", [20, 11], C.ink);
    line(ctx, [24, 40], [4, 40], C.ink, 1.5); label(ctx, "E", [4, 54], C.ink);

    var pl = sky(w + o.nu, o.r);
    var Rp = Math.max(3, 0.1 * s / st.aRs), Rs = s / st.aRs;
    if (pl.z < 0) dot(ctx, pl.p, Rp, C.planet);
    star(ctx, c, Math.max(4, Rs));
    if (pl.z >= 0) dot(ctx, pl.p, Rp, C.planet);
  }

  // Stellar disk with the projected orbit normal pointing up.
  function drawSpin(o, C, b) {
    var d = prep("ob-spin"), ctx = d.ctx, R = 0.34 * Math.min(d.w, d.h), c = [d.w / 2, d.h / 2 + 4];
    var lam = st.lam * D, is = st.istar * D, u = [Math.sin(lam), -Math.cos(lam)], p = [Math.cos(lam), Math.sin(lam)];
    star(ctx, c, R);

    // Doppler tint: one projected half approaches, the other recedes, scaled by sin i★.
    ctx.save(); ctx.beginPath(); ctx.arc(c[0], c[1], R, 0, TAU); ctx.clip();
    [[1, "rgba(220,38,38,"], [-1, "rgba(37,99,235,"]].forEach(function (h) {
      ctx.fillStyle = h[1] + (0.42 * Math.sin(is)).toFixed(3) + ")";
      ctx.beginPath();
      ctx.moveTo(c[0] - u[0] * 2 * R, c[1] - u[1] * 2 * R); ctx.lineTo(c[0] + u[0] * 2 * R, c[1] + u[1] * 2 * R);
      ctx.lineTo(c[0] + u[0] * 2 * R + h[0] * p[0] * 2 * R, c[1] + u[1] * 2 * R + h[0] * p[1] * 2 * R);
      ctx.lineTo(c[0] - u[0] * 2 * R + h[0] * p[0] * 2 * R, c[1] - u[1] * 2 * R + h[0] * p[1] * 2 * R);
      ctx.fill();
    });
    ctx.restore();

    if (Math.sin(is) > 0.15) {
      var halo = "rgba(255,255,255,0.75)", off = [u[0] * 0.45 * R, u[1] * 0.45 * R];
      label(ctx, "receding", [c[0] + p[0] * 0.5 * R + off[0], c[1] + p[1] * 0.5 * R + off[1]], "#991b1b", "center", false, halo);
      label(ctx, "approaching", [c[0] - p[0] * 0.5 * R + off[0], c[1] - p[1] * 0.5 * R + off[1]], "#1e40af", "center", false, halo);
    }
    ctx.strokeStyle = "rgba(90,60,20,0.55)"; ctx.lineWidth = 1;
    ctx.beginPath(); ctx.ellipse(c[0], c[1], R, Math.max(0.5, R * Math.abs(Math.cos(is))), lam, 0, TAU); ctx.stroke();

    line(ctx, [c[0], c[1] + 1.3 * R], [c[0], c[1] - 1.35 * R], C.muted, 1, [4, 3]);
    label(ctx, "orbit normal", [c[0] - 6, c[1] - 1.28 * R], C.muted, "right");
    var tip = [c[0] + u[0] * 1.35 * R, c[1] + u[1] * 1.35 * R];
    line(ctx, [c[0] - u[0] * 1.3 * R, c[1] - u[1] * 1.3 * R], tip, C.ink, 1.5);
    label(ctx, "spin", [tip[0] + 4, tip[1]], C.ink);
    dot(ctx, [c[0] + u[0] * R * Math.sin(is), c[1] + u[1] * R * Math.sin(is)], 3.5, C.ink, st.istar > 90);

    ctx.strokeStyle = C.accent; ctx.lineWidth = 2; ctx.beginPath();
    ctx.arc(c[0], c[1], 1.15 * R, -Math.PI / 2, lam - Math.PI / 2, lam < 0); ctx.stroke();
    label(ctx, "λ", [c[0] + 1.25 * R * Math.sin(lam / 2), c[1] - 1.25 * R * Math.cos(lam / 2)], C.accent, "center", true);

    if (Math.abs(b) < 1) {
      // For i < 90° the planet crosses on the side opposite the projected orbit normal.
      var half = Math.sqrt(1 - b * b) * R, y = c[1] + b * R;
      line(ctx, [c[0] - half, y], [c[0] + half, y], C.planet, 2);
      line(ctx, [c[0] + half - 7, y - 4], [c[0] + half, y], C.planet, 2);
      line(ctx, [c[0] + half - 7, y + 4], [c[0] + half, y], C.planet, 2);
    } else {
      label(ctx, "no transit chord (|b| ≥ 1)", [c[0], d.h - 8], C.muted, "center");
    }
  }

  function draw() {
    SPECS.forEach(function (s) { outputs[s[0]].value = st[s[0]].toFixed(s[5]); });
    var o = orbit(), C = colors(), inc = st.i * D, is = st.istar * D, lam = st.lam * D;
    var b = st.aRs * Math.cos(inc) * (1 - o.e * o.e) / (1 + o.e * Math.sin(o.w));
    drawPlane(o, C); drawSky(o, C); drawSpin(o, C, b);

    var psi = Math.acos(Math.max(-1, Math.min(1, Math.cos(is) * Math.cos(inc) + Math.sin(is) * Math.sin(inc) * Math.cos(lam))));
    var veq = TAU * st.rs * RSUN_KM / (st.prot * DAY);
    var deg = function (x) { return (x / D).toFixed(1) + "°"; };
    var rows = [
      ["Mean anomaly M", deg(o.M), COL.M],
      ["Eccentric anomaly E", deg(o.E), COL.E],
      ["True anomaly ν", deg(o.nu), COL.nu],
      ["Check: E − e sin E", deg(wrap2pi(o.E - o.e * Math.sin(o.E))), null],
      ["Distance r / a", o.r.toFixed(3), null],
      ["ν at transit = 90° − ω", deg(wrap2pi(Math.PI / 2 - o.w)), null],
      ["Impact parameter b", b.toFixed(3), null],
      ["True obliquity ψ", deg(psi), null],
      ["v_eq", veq.toFixed(2) + " km/s", null],
      ["v sin i★", (veq * Math.sin(is)).toFixed(2) + " km/s", null],
      ["Star color (main sequence)", (function () { var c = window.transitBookStarColor(st.teff); return c.spt + " · " + c.hex; })(), null]
    ];
    if (root.classList.contains("tg-focus")) rows = rows.filter(function (r) { return !NON_TRANSIT_ROWS[r[0]]; });
    var out = document.getElementById("ob-out");
    out.replaceChildren.apply(out, rows.map(function (r) {
      var div = document.createElement("div"), dt = document.createElement("dt"), dd = document.createElement("dd");
      dt.textContent = r[0]; dd.textContent = r[1]; if (r[2]) dd.style.color = r[2];
      div.append(dt, dd); return div;
    }));
    document.getElementById("ob-status").textContent =
      st.e < 0.005 ? "For a circular orbit M, E, and ν coincide and ω is undefined." : "";
  }

  window.transitBookSortable(box, "transit-book:orbit-cards", document.getElementById("ob-layout"));
  var figs = root.querySelector(".tg-figs");
  window.transitBookFigureGrips(figs);
  window.transitBookSortable(figs, "transit-book:orbit-plots", document.getElementById("ob-layout"), { item: ".tg-fig", onChange: function () { draw(); } });
  var NON_TRANSIT = { W: 1, istar: 1, lam: 1, prot: 1, rs: 1, teff: 1 };
  var NON_TRANSIT_ROWS = { "True obliquity ψ": 1, "v_eq": 1, "v sin i★": 1, "Star color (main sequence)": 1 };
  window.transitBookFocus(root, box, NON_TRANSIT, document.getElementById("ob-focus"), draw);
  linkMainSequence("rs");
  if (window.ResizeObserver) new ResizeObserver(draw).observe(root);
  new MutationObserver(draw).observe(document.documentElement, { attributes: true, attributeFilter: ["data-theme"] });
  draw();
})();
</script>
```

### What to try

The presets set up standard configurations. Tick *Only parameters that shape the transit* to hide $\Omega$ and the stellar-spin controls, which leave an ordinary transit light curve unchanged. Presets with a transiting geometry start with the planet at mid-transit, so moving the time slider shows how long it takes to reach periastron.

- **Kepler's equation.** Set $e\approx0.6$ and drag the time slider. The hollow green point (mean anomaly) moves uniformly around the auxiliary circle. The orange point (eccentric anomaly) runs ahead of it after periastron and behind it after apoastron. The planet lies where a line from the orange point, drawn perpendicular to the major axis, meets the ellipse. The check readout shows $E-e\sin E$ reproducing $M$.
- **Where transit happens.** Change $\omega$ and watch the periastron move relative to the "transit" point at the bottom of the orbit. With $\omega=90^\circ$ the planet transits at periastron, moving fastest, so its transit is shortest. With $\omega=-90^\circ$ it transits at apoastron.
- **The node is invisible.** Rotate $\Omega$. The projected orbit spins on the sky, but nothing about the transit chord or the light curve changes.
- **Projected and true obliquity.** Set $\lambda=0$ and lower $i_\star$. The projected axes stay aligned while $\psi$ grows, which is why $\lambda$ alone gives only a lower limit on misalignment. At $i_\star$ near zero the star is seen pole-on, the Doppler tint fades, and $v\sin i_\star$ drops toward zero.

## Transit geometry

The parameters in [Choosing parameters and priors](parameters-and-priors.md) are easier to reason about when you can watch them move. The panels below show one planet from above its orbital plane, the stellar disk as the observer sees it, and the light curve that results. Drag the orbital phase to move the planet, or change one parameter at a time and watch which features of the light curve respond.

The sliders in a group describe the same quantity in different coordinates, so they move together. In *Orbital geometry*, inclination, $\cos i$, and $b$ are linked by $b=(a/R_\star)\cos i\,(1-e^2)/(1+e\sin\omega)$. In *Transit timescale*, $a/R_\star$, $T_{14}$, and $\rho_\star$ are linked through the period and the geometry. In *Eccentricity*, $(e,\omega)$ and $(f_s,f_c)=(\sqrt e\sin\omega,\sqrt e\cos\omega)$ are two coordinate systems for the same orbit. The **hold** button decides which quantity in the group stays fixed when you change a parameter outside it, which is the same decision as choosing the sampled coordinate in a fit. Tick *Only parameters that shape the transit* to hide the stellar radius and temperature, which change the physical readouts and the star's color but not the light curve. Drag a parameter card or a plot panel by its ⠿ grip, or focus the grip and use the arrow keys, to rearrange the figure. For example, move the light curve to the top or a card next to it while you work with that card.

```{raw} html

<div class="tg" id="tg-root">
  <div class="tg-figs">
    <figure class="tg-fig">
      <figcaption><span>Orbit seen from above</span><span>observer ↓</span></figcaption>
      <canvas class="tg-c-sq" id="tg-top" role="img" aria-label="Top-down view of the orbit"></canvas>
    </figure>
    <figure class="tg-fig">
      <figcaption><span>Stellar disk as seen by the observer</span><span>units of R★</span></figcaption>
      <canvas class="tg-c-sq" id="tg-sky" role="img" aria-label="Sky-plane view of the transit chord"></canvas>
    </figure>
    <figure class="tg-fig tg-wide">
      <figcaption><span>Light curve</span><span></span></figcaption>
      <canvas class="tg-c-lc" id="tg-lc" role="img" aria-label="Model light curve"></canvas>
    </figure>
  </div>

  <div class="tg-bar">
    <label>Preset
      <select id="tg-preset">
        <option>Default</option><option>Hot Jupiter</option><option>Ultra-hot Jupiter</option>
        <option>Super-Earth</option><option>Earth-like</option><option>Grazing</option><option>Eccentric giant</option>
      </select>
    </label>
    <button type="button" id="tg-reset">Reset</button>
    <button type="button" id="tg-layout">Reset layout</button>
    <label><input type="checkbox" id="tg-zoom" checked> Zoom light curve on transit</label>
    <label><input type="checkbox" id="tg-focus"> Only parameters that shape the transit</label>
  </div>

  <div class="tg-controls" id="tg-controls"></div>

  <dl class="tg-out" id="tg-out" aria-live="polite"></dl>
  <p class="tg-status" id="tg-status" role="status"></p>
</div>

<script>
(function () {
  "use strict";
  var root = document.getElementById("tg-root");
  if (!root) return;

  var G_CGS = 6.6743e-8, RHO_SUN = 1.41, DAY = 86400;

  // name, label, min, max, step, log-scaled, decimals, group
  var SPECS = [
    ["phase", "Orbital phase", -0.5, 0.5, 0.001, false, 3, "orbit"],
    ["period", "Period P (days)", 0.5, 400, 0.002, true, 2, "orbit"],
    ["k", "Radius ratio k = Rp/R★", 0.005, 0.3, 0.001, false, 3, "orbit"],
    ["inc", "Inclination i (°)", 60, 90, 0.01, false, 2, "geom"],
    ["cosi", "cos i", 0, 0.5, 0.001, false, 3, "geom"],
    ["b", "Impact parameter b", 0, 1.4, 0.001, false, 3, "geom"],
    ["aRs", "Scaled semimajor axis a/R★", 1.5, 250, 0.002, true, 2, "time"],
    ["T14", "Total duration T₁₄ (hours)", 0.1, 40, 0.002, true, 2, "time"],
    ["rho", "Stellar density ρ★ (g/cm³)", 0.005, 100, 0.002, true, 3, "time"],
    ["q1", "q₁", 0, 1, 0.01, false, 2, "ld"],
    ["q2", "q₂", 0, 1, 0.01, false, 2, "ld"],
    ["ecc", "Eccentricity e", 0, 0.9, 0.001, false, 3, "ecc"],
    ["omega", "Argument of periastron ω (°)", -180, 180, 0.5, false, 1, "ecc"],
    ["fs", "f_s = √e sin ω", -0.95, 0.95, 0.01, false, 2, "ecc"],
    ["fc", "f_c = √e cos ω", -0.95, 0.95, 0.01, false, 2, "ecc"],
    ["c", "Contaminating flux fraction c", 0, 0.5, 0.01, false, 2, "dil"],
    ["cad", "Cadence (minutes)", 0.33, 60, 0.002, true, 2, "data"],
    ["sig", "Uncertainty per point σ (ppt)", 0.05, 20, 0.002, true, 2, "data"],
    ["Rs", "Stellar radius R★ (R☉)", 0.1, 10, 0.002, true, 2, "phys"],
    ["teff", "Effective temperature T_eff (K)", 2300, 55000, 0.002, true, 0, "phys"]
  ];
  var GROUPS = [
    ["orbit", "Orbit and planet"], ["geom", "Orbital geometry"], ["time", "Transit timescale"],
    ["ld", "Limb darkening"], ["ecc", "Eccentricity"], ["dil", "Dilution"], ["phys", "Physical scale"], ["data", "Simulated data"]
  ];
  var HOLD_HINT = {
    geom: "Held quantity stays fixed when parameters outside this group change. Holding i or cos i is the same constraint.",
    time: "Held quantity stays fixed when parameters outside this group change."
  };
  var hold = { geom: "cosi", time: "aRs" };
  var spec = {};
  SPECS.forEach(function (s) { spec[s[0]] = s; });

  var PRESETS = {
    "Default":           { period: 3, k: 0.1, aRs: 10, cosi: 0.05, q1: 0.5, q2: 0.3, fs: 0, fc: 0, c: 0 },
    "Hot Jupiter":       { period: 3.5, k: 0.12, aRs: 7.5, cosi: 0.02, q1: 0.35, q2: 0.25, fs: 0, fc: 0, c: 0 },
    "Ultra-hot Jupiter": { period: 1.8, k: 0.15, aRs: 4.5, cosi: 0.01, q1: 0.3, q2: 0.2, fs: 0, fc: 0, c: 0 },
    "Super-Earth":       { period: 8.5, k: 0.025, aRs: 12, cosi: 0.03, q1: 0.4, q2: 0.28, fs: 0, fc: 0, c: 0 },
    "Earth-like":        { period: 365.25, k: 0.009, aRs: 215, cosi: 0, q1: 0.4, q2: 0.26, fs: 0, fc: 0, c: 0 },
    "Grazing":           { period: 4.2, k: 0.11, aRs: 8.5, cosi: 0.12, q1: 0.38, q2: 0.32, fs: 0, fc: 0, c: 0 },
    "Eccentric giant":   { period: 25, k: 0.1, aRs: 25, cosi: 0.01, q1: 0.45, q2: 0.2, fs: 0.3, fc: 0.5, c: 0 }
  };

  // Canonical state. Every other slider is derived from these values.
  var MS = window.transitBookMainSequence, msOn = true;
  var st = { Rs: 1, teff: MS.teffFromR(1), omegaDeg: 90, cad: 2, sig: 1 }, solveNote = "";
  var dataOn = false, smearOn = true, dataSeed = 1, MAX_POINTS = 6000, SUBSAMPLES = 7;
  var RSUN_REARTH = 109.08, RSUN_RJUP = 9.731, AU_RSUN = 215.03;

  // ---------- physics ----------
  function eccOf(s) {
    var fs = s.fs, fc = s.fc, e = fs * fs + fc * fc, capped = false;
    if (e > 0.9) { var sc = Math.sqrt(0.9 / e); fs *= sc; fc *= sc; e = 0.9; capped = true; }
    var w = e > 0 ? Math.atan2(fs, fc) : Math.PI / 2;
    return { e: e, w: w, capped: capped, g: (1 - e * e) / (1 + e * Math.sin(w)), speed: Math.sqrt(1 - e * e) / (1 + e * Math.sin(w)) };
  }
  function clampCosi(x) { return Math.max(0, Math.min(0.999, x)); }
  function aRsFromRho(rho, period) { return Math.cbrt(rho * G_CGS * Math.pow(period * DAY, 2) / (3 * Math.PI)); }

  // Duration in days between contacts (sgn = +1 for T14, -1 for T23).
  function duration(aRs, cosi, k, period, E, sgn) {
    var b = aRs * cosi * E.g, x = Math.pow(1 + sgn * k, 2) - b * b;
    if (x <= 0 || (sgn < 0 && b >= 1 - k)) return 0;
    var sini = Math.sqrt(1 - cosi * cosi);
    return period / Math.PI * Math.asin(Math.min(1, Math.sqrt(x) / (aRs * sini))) * E.speed;
  }

  // A geometry rule says what stays fixed while a/R★ changes: cos i or b.
  function cosiFor(aRs, rule, E) { return clampCosi(rule.mode === "b" ? rule.val / (aRs * E.g) : rule.val); }

  // T14 decreases monotonically with a/R★ under either rule, so bisect in log a/R★.
  function aRsForT14(target, rule, E) {
    var lo = Math.log(1.2), hi = Math.log(2000);
    var T = function (lnA) { var a = Math.exp(lnA); return duration(a, cosiFor(a, rule, E), st.k, st.period, E, 1); };
    for (var i = 0; i < 60; i++) { var mid = 0.5 * (lo + hi); if (T(mid) > target) lo = mid; else hi = mid; }
    var a = Math.exp(0.5 * (lo + hi)), got = T(Math.log(a));
    solveNote = Math.abs(got - target) > 0.01 * target ? "Requested T₁₄ is not reachable with this geometry; using the closest a/R★. " : "";
    return a;
  }

  function solveKepler(M, e) {
    M = Math.atan2(Math.sin(M), Math.cos(M));
    var E = e < 0.8 ? M : Math.PI * Math.sign(M || 1);
    for (var i = 0; i < 50; i++) {
      var d = (E - e * Math.sin(E) - M) / (1 - e * Math.cos(E));
      E -= d;
      if (Math.abs(d) < 1e-12) break;
    }
    return E;
  }

  // Planet position for mean anomaly M. Returns sky-plane X (along motion), Y (perpendicular),
  // Z (> 0 toward observer), plus in-plane coordinates for the top-down view.
  function position(M, sys) {
    var e = sys.e, E = solveKepler(M, e);
    var nu = 2 * Math.atan2(Math.sqrt(1 + e) * Math.sin(E / 2), Math.sqrt(1 - e) * Math.cos(E / 2));
    var r = sys.aRs * (1 - e * e) / (1 + e * Math.cos(nu));
    var th = nu + sys.w;
    var X = -r * Math.cos(th), depthCoord = r * Math.sin(th);
    return { X: X, Y: depthCoord * sys.cosi, Z: depthCoord * sys.sini, inPlaneZ: depthCoord, nu: nu };
  }
  function meanAnomalyAtPhase(phase, sys) { return sys.Mtr + 2 * Math.PI * phase; }

  // Fraction of stellar flux blocked by a dark disk of radius k at separation z (quadratic LD).
  function blocked(z, k, u1, u2) {
    if (z >= 1 + k) return 0;
    var r0 = Math.max(0, z - k), r1 = Math.min(1, z + k), n = 240, dr = (r1 - r0) / n, sum = 0;
    for (var j = 0; j < n; j++) {
      var r = r0 + (j + 0.5) * dr, L;
      if (r <= k - z) L = 2 * Math.PI * r;
      else {
        var ca = (r * r + z * z - k * k) / (2 * r * z);
        L = 2 * r * Math.acos(Math.max(-1, Math.min(1, ca)));
      }
      var mu = Math.sqrt(1 - r * r);
      sum += (1 - u1 * (1 - mu) - u2 * (1 - mu) * (1 - mu)) * L * dr;
    }
    return sum / (Math.PI * (1 - u1 / 3 - u2 / 6));
  }

  function system() {
    var E = eccOf(st), p = st;
    var ftr = Math.PI / 2 - E.w;
    var Etr = 2 * Math.atan(Math.sqrt((1 - E.e) / (1 + E.e)) * Math.tan(ftr / 2));
    var sys = {
      p: p, e: E.e, w: E.w, aRs: p.aRs, cosi: p.cosi, sini: Math.sqrt(1 - p.cosi * p.cosi),
      u1: 2 * Math.sqrt(p.q1) * p.q2, u2: Math.sqrt(p.q1) * (1 - 2 * p.q2),
      Mtr: Etr - E.e * Math.sin(Etr), note: (E.capped ? "Eccentricity capped at 0.9. " : "") + solveNote
    };
    if (p.aRs * (1 - E.e) <= 1 + p.k) sys.note += "Periastron lies inside the star. ";
    sys.b = p.aRs * p.cosi * E.g;
    sys.T14 = duration(p.aRs, p.cosi, p.k, p.period, E, 1);
    sys.T23 = duration(p.aRs, p.cosi, p.k, p.period, E, -1);
    sys.rho = 3 * Math.PI * Math.pow(p.aRs, 3) / (G_CGS * Math.pow(p.period * DAY, 2));
    return sys;
  }

  function fluxAtPhase(phase, sys) {
    var q = position(meanAnomalyAtPhase(phase, sys), sys);
    var z = Math.hypot(q.X, q.Y);
    var drop = q.Z > 0 ? blocked(z, sys.p.k, sys.u1, sys.u2) : 0;
    return (1 - sys.p.c) * (1 - drop) + sys.p.c;
  }

  // ---------- linked updates ----------
  function onSlider(name, v) {
    var before = system(), group = spec[name][7], rule;
    solveNote = "";
    if (name === "inc") st.cosi = clampCosi(Math.cos(v * Math.PI / 180));
    else if (name === "cosi") st.cosi = clampCosi(v);
    else if (name === "b") st.cosi = clampCosi(v / (st.aRs * eccOf(st).g));
    else if (name === "aRs") st.aRs = v;
    else if (name === "rho") st.aRs = aRsFromRho(v, st.period);
    else if (name === "ecc" || name === "omega") {
      if (name === "omega") st.omegaDeg = v;
      var eNew = name === "ecc" ? v : eccOf(st).e, wr = st.omegaDeg * Math.PI / 180;
      st.fs = Math.sqrt(eNew) * Math.sin(wr); st.fc = Math.sqrt(eNew) * Math.cos(wr);
    }
    else if (name !== "T14") st[name] = v;
    if (msOn && name === "Rs") st.teff = MS.teffFromR(st.Rs);
    if (msOn && name === "teff") st.Rs = MS.rFromTeff(st.teff);
    if ((name === "fs" || name === "fc") && st.fs * st.fs + st.fc * st.fc > 0) st.omegaDeg = Math.atan2(st.fs, st.fc) * 180 / Math.PI;

    var E = eccOf(st);
    if (group === "geom") rule = name === "b" ? { mode: "b", val: v } : { mode: "cosi", val: st.cosi };
    else rule = hold.geom === "b" ? { mode: "b", val: before.b } : { mode: "cosi", val: before.cosi };

    if (name === "T14") st.aRs = aRsForT14(v / 24, rule, E);
    else if (group !== "time") {
      if (hold.time === "rho") st.aRs = aRsFromRho(before.rho, st.period);
      else if (hold.time === "T14" && before.T14 > 0) st.aRs = aRsForT14(before.T14, rule, E);
    }
    st.cosi = cosiFor(st.aRs, rule, E);
    draw();
  }

  // ---------- simulated data ----------
  // Deterministic white noise keyed to the cadence index, so points stay put while other sliders move.
  function hash01(x) {
    var h = Math.imul((x | 0) ^ Math.imul(dataSeed, 0x9E3779B1), 0x85EBCA6B);
    h ^= h >>> 13; h = Math.imul(h, 0xC2B2AE35); h ^= h >>> 16;
    return ((h >>> 0) + 0.5) / 4294967296;
  }
  function gauss(n) { return Math.sqrt(-2 * Math.log(hash01(2 * n + 1))) * Math.cos(2 * Math.PI * hash01(2 * n + 2)); }

  function simulate(sys, window_) {
    var P = sys.p.period, dt = st.cad / 1440, sigma = st.sig / 1000;
    var n0 = Math.ceil(window_[0] * P / dt), n1 = Math.floor(window_[1] * P / dt);
    if (n1 - n0 + 1 > MAX_POINTS) return { tooMany: true, count: n1 - n0 + 1 };
    var pts = [], nIn = 0;
    for (var n = n0; n <= n1; n++) {
      var t = n * dt, model = 0;
      if (smearOn) {
        for (var k = 0; k < SUBSAMPLES; k++) model += fluxAtPhase((t + ((k + 0.5) / SUBSAMPLES - 0.5) * dt) / P, sys);
        model /= SUBSAMPLES;
      } else model = fluxAtPhase(t / P, sys);
      if (model < 1 - 1e-9) nIn++;
      pts.push([t, model + sigma * gauss(n)]);
    }
    return { pts: pts, nIn: nIn, sigma: sigma };
  }

  // ---------- controls ----------
  var inputs = {}, outputs = {};
  var box = document.getElementById("tg-controls");
  GROUPS.forEach(function (g) {
    var fs = document.createElement("fieldset");
    fs.className = "tg-group"; fs.id = "tg-group-" + g[0];
    var lg = document.createElement("legend");
    fs.dataset.key = g[0];
    window.transitBookCardTitle(lg, g[1]);
    fs.appendChild(lg);
    if (hold[g[0]]) {
      var hint = document.createElement("p");
      hint.className = "tg-hint"; hint.textContent = HOLD_HINT[g[0]];
      fs.appendChild(hint);
    }
    SPECS.filter(function (s) { return s[7] === g[0]; }).forEach(function (s) {
      var id = "tg-in-" + s[0];
      var wrap = document.createElement("div");
      wrap.className = "tg-ctl";
      var lab = document.createElement("label");
      lab.htmlFor = id; lab.textContent = s[1];
      var out = document.createElement("output");
      out.htmlFor = id;
      wrap.append(lab, out);
      if (hold[g[0]]) {
        var hl = document.createElement("label");
        hl.className = "tg-hold";
        var radio = document.createElement("input");
        radio.type = "radio"; radio.name = "tg-hold-" + g[0]; radio.value = s[0];
        radio.checked = hold[g[0]] === s[0];
        radio.setAttribute("aria-label", "Hold " + s[1] + " fixed");
        radio.addEventListener("change", function () { hold[g[0]] = s[0]; });
        hl.append(radio, document.createTextNode("hold"));
        wrap.appendChild(hl);
      } else {
        wrap.appendChild(document.createElement("span"));
      }
      var inp = document.createElement("input");
      inp.type = "range"; inp.id = id;
      inp.min = s[5] ? Math.log10(s[2]) : s[2];
      inp.max = s[5] ? Math.log10(s[3]) : s[3];
      inp.step = s[4];
      inp.addEventListener("input", function () {
        var raw = parseFloat(inp.value);
        onSlider(s[0], s[5] ? Math.pow(10, raw) : raw);
      });
      wrap.appendChild(inp);
      fs.appendChild(wrap);
      inputs[s[0]] = inp; outputs[s[0]] = out;
    });
    box.appendChild(fs);
  });

  (function addMainSequenceToggle() {
    var grp = document.getElementById("tg-group-phys"), row = document.createElement("div");
    row.className = "tg-bar tg-data-bar";
    var lab = document.createElement("label"), cb = document.createElement("input");
    cb.type = "checkbox"; cb.checked = msOn;
    cb.addEventListener("change", function () { msOn = cb.checked; if (msOn) st.teff = MS.teffFromR(st.Rs); draw(); });
    lab.append(cb, document.createTextNode(" Main-sequence star: link T_eff and R★"));
    row.appendChild(lab); grp.insertBefore(row, grp.children[1]);
  })();

  (function addDataToggles() {
    var grp = document.getElementById("tg-group-data"), row = document.createElement("div");
    row.className = "tg-bar tg-data-bar";
    var mk = function (id, text, checked) {
      var lab = document.createElement("label"), cb = document.createElement("input");
      cb.type = "checkbox"; cb.id = id; cb.checked = checked;
      lab.append(cb, document.createTextNode(" " + text)); row.appendChild(lab); return cb;
    };
    mk("tg-data", "Show simulated data", dataOn).addEventListener("change", function (ev) { dataOn = ev.target.checked; draw(); });
    mk("tg-smear", "Average model over each exposure", smearOn).addEventListener("change", function (ev) { smearOn = ev.target.checked; draw(); });
    var btn = document.createElement("button");
    btn.type = "button"; btn.textContent = "New noise";
    btn.addEventListener("click", function () { dataSeed = (dataSeed * 1103515245 + 12345) >>> 0; draw(); });
    row.appendChild(btn);
    grp.insertBefore(row, grp.children[1]);
  })();

  function setSlider(name, v, text) {
    var s = spec[name];
    inputs[name].value = s[5] ? Math.log10(Math.max(v, 1e-9)) : v;
    outputs[name].value = text != null ? text : v.toFixed(s[6]);
  }

  function loadPreset(name) {
    var p = PRESETS[name];
    Object.keys(p).forEach(function (key) { st[key] = p[key]; });
    st.phase = 0; solveNote = "";
    if (st.fs * st.fs + st.fc * st.fc > 0) st.omegaDeg = Math.atan2(st.fs, st.fc) * 180 / Math.PI;
    draw();
  }

  // ---------- drawing ----------
  function colors() {
    var cs = getComputedStyle(root);
    var g = function (n) { return cs.getPropertyValue(n).trim(); };
    return { ink: g("--tg-ink"), muted: g("--tg-muted"), line: g("--tg-line"), accent: g("--tg-accent"), planet: g("--tg-planet"), warn: g("--tg-warn") };
  }

  function prep(canvas) {
    var dpr = window.devicePixelRatio || 1, w = canvas.clientWidth, h = canvas.clientHeight;
    if (canvas.width !== Math.round(w * dpr) || canvas.height !== Math.round(h * dpr)) {
      canvas.width = Math.round(w * dpr); canvas.height = Math.round(h * dpr);
    }
    var ctx = canvas.getContext("2d");
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    ctx.clearRect(0, 0, w, h);
    return { ctx: ctx, w: w, h: h };
  }

  function starFill(ctx, cx, cy, R, sys) {
    return window.transitBookStarGradient(ctx, cx, cy, R, st.teff, sys.u1, sys.u2);
  }

  function outline(ctx, x, y, r) {
    ctx.beginPath(); ctx.arc(x, y, r, 0, 2 * Math.PI); ctx.strokeStyle = "rgba(0,0,0,0.25)"; ctx.lineWidth = 1; ctx.stroke();
  }
  function disk(ctx, x, y, r, fill, alpha) {
    ctx.globalAlpha = alpha == null ? 1 : alpha;
    ctx.beginPath(); ctx.arc(x, y, r, 0, 2 * Math.PI); ctx.fillStyle = fill; ctx.fill();
    ctx.globalAlpha = 1;
  }

  function drawTop(sys, phase, C) {
    var d = prep(document.getElementById("tg-top")), ctx = d.ctx;
    var e = sys.e, a = sys.aRs, pts = [], i;
    for (i = 0; i <= 360; i++) pts.push(position(i / 360 * 2 * Math.PI, sys));
    var ext = a * (1 + e) * 1.12;
    var s = Math.min(d.w, d.h) / (2 * ext), cx = d.w / 2, cy = d.h / 2;
    var P = function (q) { return [cx + q.X * s, cy + q.inPlaneZ * s]; };

    ctx.strokeStyle = C.line; ctx.lineWidth = 1; ctx.setLineDash([3, 4]);
    ctx.beginPath(); ctx.moveTo(cx, cy); ctx.lineTo(cx, d.h); ctx.stroke(); ctx.setLineDash([]);

    ctx.strokeStyle = C.muted; ctx.lineWidth = 1.5; ctx.beginPath();
    pts.forEach(function (q, j) { var xy = P(q); j ? ctx.lineTo(xy[0], xy[1]) : ctx.moveTo(xy[0], xy[1]); });
    ctx.stroke();

    disk(ctx, cx, cy, Math.max(3, s), starFill(ctx, cx, cy, Math.max(3, s), sys));
    outline(ctx, cx, cy, Math.max(3, s));

    if (e > 0.02) {
      [[0, "#c2410c", "periastron"], [Math.PI, "#15803d", "apoastron"]].forEach(function (m) {
        var xy = P(position(m[0], sys));
        disk(ctx, xy[0], xy[1], 4, m[1]);
        ctx.fillStyle = C.muted; ctx.font = "11px sans-serif";
        var right = xy[0] > d.w / 2;
        ctx.textAlign = right ? "right" : "left";
        ctx.fillText(m[2], xy[0] + (right ? -6 : 6), xy[1] - 6);
        ctx.textAlign = "left";
      });
    }
    var q = P(position(meanAnomalyAtPhase(phase, sys), sys));
    disk(ctx, q[0], q[1], Math.max(3.5, sys.p.k * s), C.planet);
  }

  function drawSky(sys, phase, window_, C) {
    var d = prep(document.getElementById("tg-sky")), ctx = d.ctx;
    var k = sys.p.k, ext = 1.6 + k;
    var s = Math.min(d.w, d.h) / (2 * ext), cx = d.w / 2, cy = d.h / 2;
    var now = position(meanAnomalyAtPhase(phase, sys), sys);
    var P = function (q) { return [cx + q.X * s, cy - q.Y * s]; };

    if (now.Z <= 0) { var b0 = P(now); disk(ctx, b0[0], b0[1], k * s, C.planet, 0.35); }
    disk(ctx, cx, cy, s, starFill(ctx, cx, cy, s, sys));
    outline(ctx, cx, cy, s);

    ctx.strokeStyle = C.line; ctx.setLineDash([2, 3]); ctx.lineWidth = 1;
    ctx.beginPath(); ctx.moveTo(cx - ext * s, cy); ctx.lineTo(cx + ext * s, cy); ctx.stroke();
    ctx.setLineDash([]);

    // transit chord over the plotted window, front side only
    ctx.beginPath();
    var started = false;
    for (var i = 0; i <= 200; i++) {
      var ph = window_[0] + (window_[1] - window_[0]) * i / 200;
      var q = position(meanAnomalyAtPhase(ph, sys), sys);
      if (q.Z <= 0 || Math.abs(q.X) > ext * 1.5) { started = false; continue; }
      var xy = P(q);
      started ? ctx.lineTo(xy[0], xy[1]) : ctx.moveTo(xy[0], xy[1]);
      started = true;
    }
    // A light halo under the chord keeps it visible against any stellar color.
    ctx.strokeStyle = "rgba(255,255,255,0.75)"; ctx.lineWidth = 3.5; ctx.stroke();
    ctx.strokeStyle = C.planet; ctx.lineWidth = 1.5; ctx.stroke();

    var mid = P(position(sys.Mtr, sys));
    if (Math.abs(sys.b) < ext) {
      ctx.strokeStyle = C.accent; ctx.lineWidth = 1;
      ctx.beginPath(); ctx.moveTo(cx, cy); ctx.lineTo(mid[0], mid[1]); ctx.stroke();
      ctx.fillStyle = C.ink; ctx.font = "12px sans-serif";
      ctx.fillText("b = " + sys.b.toFixed(2), cx + s + 6, cy - 6);
    }
    if (now.Z > 0) {
      // A transiting planet is an opaque silhouette; a light rim separates it from the stellar disk.
      var xy2 = P(now), rp = Math.max(1.5, k * s);
      disk(ctx, xy2[0], xy2[1], rp, "#111827");
      ctx.beginPath(); ctx.arc(xy2[0], xy2[1], rp, 0, 2 * Math.PI);
      ctx.strokeStyle = "rgba(255,255,255,0.85)"; ctx.lineWidth = 1.2; ctx.stroke();
    }
  }

  function drawLC(sys, phase, window_, C) {
    var d = prep(document.getElementById("tg-lc")), ctx = d.ctx;
    var P = sys.p.period, zoom = window_[1] - window_[0] < 1;
    var toX = zoom ? 24 * P : P, xlabel = "Time from mid-transit (" + (zoom ? "hours" : "days") + ")";
    var n = 500, ts = [], fs = [], i;
    for (i = 0; i <= n; i++) {
      var ph = window_[0] + (window_[1] - window_[0]) * i / n;
      ts.push(ph * toX); fs.push(fluxAtPhase(ph, sys));
    }
    var fmin = Math.min.apply(null, fs), span = Math.max(1 - fmin, 2e-4);
    var y0 = fmin - 0.15 * span, y1 = 1 + 0.15 * span;
    var sim = dataOn ? simulate(sys, window_) : null;
    if (sim && sim.pts) { y0 = Math.min(y0, fmin - 3 * sim.sigma); y1 = Math.max(y1, 1 + 3 * sim.sigma); }
    var L = 74, R = 12, T = 8, B = 42, pw = d.w - L - R, phh = d.h - T - B;
    var X = function (t) { return L + (t - ts[0]) / (ts[n] - ts[0]) * pw; };
    var Y = function (f) { return T + (y1 - f) / (y1 - y0) * phh; };

    ctx.strokeStyle = C.line; ctx.lineWidth = 1; ctx.strokeRect(L, T, pw, phh);
    ctx.fillStyle = C.muted; ctx.font = "11px sans-serif";
    [y0 + 0.15 * span, 1].forEach(function (f) {
      ctx.textAlign = "right"; ctx.fillText(f.toFixed(4), L - 6, Y(f) + 4); ctx.textAlign = "left";
      ctx.beginPath(); ctx.moveTo(L, Y(f)); ctx.lineTo(L + pw, Y(f)); ctx.globalAlpha = 0.5; ctx.stroke(); ctx.globalAlpha = 1;
    });
    ctx.textAlign = "center";
    [ts[0], 0, ts[n]].forEach(function (t) { ctx.fillText(t.toFixed(t === 0 ? 0 : 2), X(t), T + phh + 15); });
    ctx.fillStyle = C.ink; ctx.font = "12px sans-serif";
    ctx.fillText(xlabel, L + pw / 2, d.h - 6);
    ctx.save(); ctx.translate(14, T + phh / 2); ctx.rotate(-Math.PI / 2); ctx.fillText("Relative flux", 0, 0); ctx.restore();
    ctx.textAlign = "left"; ctx.font = "11px sans-serif";

    if (sim && sim.pts) {
      ctx.save(); ctx.beginPath(); ctx.rect(L, T, pw, phh); ctx.clip();
      var dense = sim.pts.length > 1500, rad = dense ? 1.1 : 1.8, cap = dense ? 0 : 2;
      // ±1σ error bars, drawn lighter than the points so dense data stays readable.
      ctx.strokeStyle = C.muted; ctx.lineWidth = 1; ctx.globalAlpha = dense ? 0.12 : 0.35;
      ctx.beginPath();
      sim.pts.forEach(function (q) {
        var x = X(q[0] / sys.p.period * toX), yHi = Y(q[1] + sim.sigma), yLo = Y(q[1] - sim.sigma);
        ctx.moveTo(x, yHi); ctx.lineTo(x, yLo);
        if (cap) { ctx.moveTo(x - cap, yHi); ctx.lineTo(x + cap, yHi); ctx.moveTo(x - cap, yLo); ctx.lineTo(x + cap, yLo); }
      });
      ctx.stroke();
      ctx.fillStyle = C.muted; ctx.globalAlpha = dense ? 0.35 : 0.75;
      sim.pts.forEach(function (q) { ctx.beginPath(); ctx.arc(X(q[0] / sys.p.period * toX), Y(q[1]), rad, 0, 2 * Math.PI); ctx.fill(); });
      ctx.restore();
    }

    ctx.strokeStyle = C.planet; ctx.lineWidth = 2; ctx.beginPath();
    for (i = 0; i <= n; i++) { i ? ctx.lineTo(X(ts[i]), Y(fs[i])) : ctx.moveTo(X(ts[i]), Y(fs[i])); }
    ctx.stroke();

    if (phase >= window_[0] && phase <= window_[1]) {
      var tx = X(phase * toX);
      ctx.strokeStyle = C.accent; ctx.setLineDash([4, 3]); ctx.lineWidth = 1;
      ctx.beginPath(); ctx.moveTo(tx, T); ctx.lineTo(tx, T + phh); ctx.stroke(); ctx.setLineDash([]);
      disk(ctx, tx, Y(fluxAtPhase(phase, sys)), 4, C.accent);
    }
    return { depth: 1 - fmin, sim: sim };
  }

  function fmtHours(days) { return days > 0 ? (24 * days).toFixed(2) + " h" : "none"; }

  function syncSliders(sys) {
    ["phase", "period", "k", "q1", "q2", "fs", "fc", "c", "aRs", "cosi", "Rs", "teff", "cad", "sig"].forEach(function (n) { setSlider(n, st[n]); });
    setSlider("ecc", sys.e);
    setSlider("omega", st.omegaDeg, sys.e > 0 ? null : st.omegaDeg.toFixed(1) + " (undef.)");
    setSlider("inc", Math.acos(st.cosi) * 180 / Math.PI);
    setSlider("b", sys.b);
    setSlider("rho", sys.rho);
    if (sys.T14 > 0) setSlider("T14", 24 * sys.T14); else setSlider("T14", spec.T14[2], "none");
  }

  function draw() {
    var sys = system(), phase = st.phase, C = colors();
    syncSliders(sys);
    var zoom = document.getElementById("tg-zoom").checked && sys.T14 > 0;
    var half = Math.min(0.5, Math.max(1.5 * sys.T14 / sys.p.period, 0.01));
    var window_ = zoom ? [-half, half] : [-0.5, 0.5];

    drawTop(sys, phase, C);
    drawSky(sys, phase, window_, C);
    var lc = drawLC(sys, phase, window_, C), depth = lc.depth, sim = lc.sim;

    var nu = position(meanAnomalyAtPhase(phase, sys), sys).nu;
    var rows = [
      ["Depth", (1e3 * depth).toFixed(2) + " ppt"],
      ["Full duration T₂₃", fmtHours(sys.T23)],
      ["Ingress fraction (T₁₄−T₂₃)/T₁₄", sys.T14 > 0 ? ((sys.T14 - sys.T23) / sys.T14).toFixed(3) : "—"],
      ["Ingress duration", sys.T14 > 0 ? (12 * (sys.T14 - sys.T23) * 60).toFixed(1) + " min" : "—"],
      ["Stellar density / solar", (sys.rho / RHO_SUN).toFixed(2) + " ρ☉"],
      ["True anomaly ν", (((nu * 180 / Math.PI) % 360 + 360) % 360).toFixed(1) + "°"],
      ["u₁, u₂", sys.u1.toFixed(2) + ", " + sys.u2.toFixed(2)],
      ["Planet radius Rp", (sys.p.k * st.Rs * RSUN_REARTH).toFixed(2) + " R⊕ (" + (sys.p.k * st.Rs * RSUN_RJUP).toFixed(3) + " R_J)"],
      ["Semimajor axis a", (sys.aRs * st.Rs / AU_RSUN).toFixed(4) + " AU"],
      ["Stellar mass M★", (sys.rho / RHO_SUN * Math.pow(st.Rs, 3)).toFixed(2) + " M☉"],
      ["Star color (main sequence)", (function () { var c = window.transitBookStarColor(st.teff); return c.spt + " · " + c.hex; })()]
    ];
    if (sim && sim.pts) {
      rows.push(["Points shown", String(sim.pts.length)]);
      rows.push(["In-transit points N_in", String(sim.nIn)]);
      rows.push(["Depth S/N ≈ δ/σ·√N_in", sim.nIn ? (depth / sim.sigma * Math.sqrt(sim.nIn)).toFixed(1) : "—"]);
    }
    if (root.classList.contains("tg-focus")) rows = rows.filter(function (r) { return !NON_TRANSIT_ROWS[r[0]]; });
    var out = document.getElementById("tg-out");
    out.replaceChildren.apply(out, rows.map(function (r) {
      var div = document.createElement("div"), dt = document.createElement("dt"), dd = document.createElement("dd");
      dt.textContent = r[0]; dd.textContent = r[1]; div.append(dt, dd); return div;
    }));

    var msg = sys.note;
    if (sim && sim.tooMany) msg += "Too many cadences to plot (" + sim.count + "); zoom on the transit or lengthen the cadence. ";
    if (sys.b >= 1 + sys.p.k) msg += "No transit: b ≥ 1 + k.";
    else if (sys.T23 === 0) msg += "Grazing transit: the planet never lies fully inside the stellar disk.";
    document.getElementById("tg-status").textContent = msg;
  }

  document.getElementById("tg-preset").addEventListener("change", function (ev) { loadPreset(ev.target.value); });
  document.getElementById("tg-reset").addEventListener("click", function () {
    document.getElementById("tg-preset").value = "Default"; loadPreset("Default");
  });
  document.getElementById("tg-zoom").addEventListener("change", draw);
  if (window.ResizeObserver) new ResizeObserver(draw).observe(root);
  new MutationObserver(draw).observe(document.documentElement, { attributes: true, attributeFilter: ["data-theme"] });
  window.transitBookSortable(box, "transit-book:transit-cards", document.getElementById("tg-layout"));
  var figs = root.querySelector(".tg-figs");
  window.transitBookFigureGrips(figs);
  window.transitBookSortable(figs, "transit-book:transit-plots", document.getElementById("tg-layout"), { item: ".tg-fig", onChange: function () { draw(); } });
  var NON_TRANSIT = { Rs: 1, teff: 1 };
  var NON_TRANSIT_ROWS = { "Planet radius Rp": 1, "Semimajor axis a": 1, "Stellar mass M★": 1, "Star color (main sequence)": 1 };
  window.transitBookFocus(root, box, NON_TRANSIT, document.getElementById("tg-focus"), draw);
  loadPreset("Default");
})();
</script>
```

### What to try

- **Impact parameter.** Raise $\cos i$ from zero. The chord moves toward the limb, the transit shortens, ingress and egress lengthen relative to the total duration, and the bottom flattens because the chord samples a narrower range of limb darkening. Past $b=1-k$ the transit becomes grazing and $T_{23}$ disappears.
- **Radius ratio and limb darkening.** Change $k$ and the depth scales roughly as $k^2$. Then set $q_1=0$ and the bottom becomes flat. With limb darkening on, the central depth exceeds $k^2$ for a central transit, as described by Heller (2019).
- **Density and duration.** At fixed period, a larger $a/R_\star$ means a denser star and a shorter transit. Compare the stellar density readout with the Sun's 1.41 g/cm³ when judging whether a preset is self-consistent.
- **Eccentricity.** Load *Eccentric giant*. The top-down view marks periastron and apoastron. Change the sign of $f_s$ to move transit toward periastron or apoastron and watch $T_{14}$ change at fixed $a/R_\star$ and $b$. This is the photoeccentric effect from [Choosing parameters and priors](parameters-and-priors.md).
- **Which coordinate is held.** Hold $\cos i$ and increase $a/R_\star$: $b$ grows, and the transit becomes shorter and more V-shaped. Now hold $b$ and repeat: only the duration shrinks. Then hold $T_{14}$ and change $b$. To keep the duration fixed, $a/R_\star$ and $\rho_\star$ must fall as the chord moves toward the limb. This is the degeneracy an independent stellar density breaks.
- **Eccentricity at fixed density.** Hold $\rho_\star$ and change $f_s$. The duration changes while the star stays the same. Hold $T_{14}$ instead and the inferred density moves, which is how the circular pseudo-density misleads when the orbit is eccentric.
- **Star color.** Move $T_{\rm eff}$ from 2300 K to 30,000 K. The disk runs from orange through near-white around spectral type F9 to pale blue, following the main-sequence colors of [Harre & Heller (2021)](https://arxiv.org/abs/2101.06254). There are no green or purple stars, and M dwarfs look orange rather than red. With *Main-sequence star* ticked, $T_{\rm eff}$ and $R_\star$ move together along the mean dwarf sequence of Pecaut & Mamajek (2013), so a star larger than about 1.2 $R_\odot$ (F8V, 6180 K) turns bluish. Untick it to set them separately, for example for an evolved host whose radius is large for its temperature.
- **Stellar radius.** The light curve depends only on ratios, so moving $R_\star$ changes nothing in the plots. It only converts $k$, $a/R_\star$, and $\rho_\star$ into a planet radius, an orbital distance, and a stellar mass.
- **Simulated data.** Tick *Show simulated data*. Lengthen the cadence to 30 minutes and the averaged model rounds off ingress and egress, so a fit that ignores exposure time would favor a grazing geometry. Compare the cadence with the *Ingress duration* readout. Raise $\sigma$ until the transit sinks into the noise, and watch the depth signal-to-noise ratio fall. At fixed total observing time, halving the cadence doubles $N_{\rm in}$ but, for photon-limited data, raises $\sigma$ by $\sqrt2$, leaving the signal-to-noise ratio unchanged.
- **Dilution.** Increase the contaminating flux fraction $c$. The transit keeps its shape but becomes shallower by a factor $1-c$, so a fit that ignores dilution underestimates $k$.

### How the figure is computed

The orbit uses the conventions of the rest of this book. The code converts $(f_s, f_c)$ to $(e,\omega)$, puts mid-transit at true anomaly $\pi/2-\omega$, and solves Kepler's equation at each phase. The impact parameter is $b=(a/R_\star)\cos i\,(1-e^2)/(1+e\sin\omega)$. Durations follow Winn (2010, equations 14–16). Setting $T_{14}$ solves for $a/R_\star$ by bisection, since the duration decreases monotonically with $a/R_\star$ whether $b$ or $\cos i$ is held. Setting $\rho_\star$ inverts Kepler's third law at fixed period. The stellar density assumes a planet of negligible mass. Quadratic coefficients come from $q_1,q_2$ by Kipping's transformation. The light curve integrates a quadratic limb-darkening profile over the area covered by an opaque planet. It ignores the planet's own light, so secondary eclipses are not shown. With the main-sequence option on, $T_{\rm eff}$ follows $R_\star$ by interpolating Mamajek's mean dwarf table (version 2022.04.16) in $\log T_{\rm eff}$ and $\log R_\star$. Disk colors interpolate the main-sequence table of Harre & Heller (2021), computed from PHOENIX and TLUSTY model spectra with CIE color-matching functions, and are darkened toward the limb with the chosen coefficients. They show chromaticity only, not brightness, and their appearance depends on your display. The plotted model curve is instantaneous. Simulated points are spaced at the chosen cadence, with exposure time equal to cadence. Each point averages the model over seven sub-exposures when that option is on, then adds independent Gaussian noise with the chosen σ, which is also drawn as each point's ±1σ error bar. The depth signal-to-noise readout is the white-noise estimate $\delta\sqrt{N_{\rm in}}/\sigma$, which correlated noise would reduce.

This page was adapted from an interactive Jupyter notebook that uses ellc and ipywidgets. Those widgets need a running Python kernel, so the static book redoes the same calculations in the browser.
