# Caribe Solar Sizer

A single-file planning tool for an expandable, battery-backed solar power backup
system on the Colombian Caribbean coast. Open `index.html` in any browser — no
build step, no dependencies.

**Live tool:** https://claude.ai/code/artifact/2b683e16-7f3c-45c6-ab48-582497c4cf92

## What it does

Enter a usable storage target (default **12 kWh**) and the loads it has to carry,
and the tool sizes the whole system and prices it:

- **Panel count** and array size in kWp, from daily energy demand and site irradiance
- **Battery bank** in 5.12 kWh / 48 V LiFePO₄ modules, sized off depth of discharge
- **Inverter model**, chosen from a catalogue of nine 48 V hybrids sold in
  Colombia: the cheapest single unit (or parallel set) that simultaneously
  clears the continuous load with 25% headroom, the compressor inrush peak, and
  the array's kWp against the MPPT input limit
- **Capital cost in COP**, broken down by category with a full bill of materials
- **Build schedule** as a dependency-ordered Gantt with a computed critical path
- **Phase 1 → final form** comparison at a configurable multiplier (default 2.5×)

It also runs three sanity checks on every recalculation — generation margin,
night-time autonomy, and inverter headroom — and says plainly when the storage
target does not cover the requested AC run hours.

## The reference design

Sized to run **2 × Midea Solstice EZ-18RD6** split units (18,000 BTU/h, 5.3 kW
cooling, R32 full-DC inverter, cooling input 634–2,120 W, SEER 8.5), expandable
to 2.5× that load.

With the default inputs — 12 kWh usable, 2 AC units at 8 h/day, Santa Marta
irradiance — Phase 1 lands at roughly **9 panels / 5.3 kWp, 15.4 kWh of LiFePO₄,
one 6 kW Growatt SPF 6000T, ≈$49.8M COP, ≈5.6 weeks**. The same inverter model
parallels to three units (18 kW) for the final form, so the platform is not
thrown away when the system grows.

The UI is in Spanish: five plain-language questions carry the basic path, and
everything else sits behind an "Ajustes avanzados" section where each field
explains what it means and why its default is what it is.

## Inverter selection

The inverter is not a fixed size. Every recalculation measures all nine
catalogued models against three independent limits and takes the cheapest that
passes all of them:

| Limit | Where it comes from |
|---|---|
| Continuous AC output | simultaneous load x 1.25 headroom |
| Surge tolerance | every compressor starting at once (2,120 W each) |
| Max PV input | the array's kWp vs. what the MPPTs accept |

The catalogue covers Growatt SPF (5/6/10 kW), Deye SUN-SG01/SG02 (5/6/8/10/12 kW)
and Felicity IVGM (8 kW), each with its real continuous rating, PV input ceiling,
surge tolerance and parallel limit. Per-unit prices are editable, and changing
one re-runs the choice.

The section shows every candidate with its verdict — chosen, viable but dearer,
or rejected naming the limit it failed — so the decision is auditable rather
than asserted. It also reports whether the Phase 1 platform can be paralleled up
to the final form or would have to be replaced, and an advanced toggle
("comprar pensando en la forma final") biases the choice toward a platform that
scales.

With the default load this picks a single 6 kW Growatt SPF 6000T at ~$7.2M COP
rather than the 8 kW unit the earlier fixed sizing assumed, cutting roughly
$2.3M while still clearing all three limits.

Victron MultiPlus units are deliberately excluded: they have no built-in PV
input, so they would need a separate MPPT charge controller and are not
comparable on a single price line.

## Model assumptions

| Parameter | Default | Basis |
|---|---|---|
| Peak sun hours | 5.5 kWh/m²·day | Santa Marta, IDEAM/UPME solar atlas — Colombia's highest band |
| Performance ratio | 78% | Soiling, cell temperature, battery round-trip |
| Depth of discharge | 90% | LiFePO₄ |
| Panel price | 889 COP/Wp | ≈$520k for a 585 Wp module (market $400k–750k) |
| Battery module | $6.5M COP | 5.12 kWh, 48 V, 100 Ah rack mount |
| Hybrid inverter | catalogue | nine real 48 V models, $5.9M-$13.9M COP, chosen by fit |
| Labour | 15% of equipment | Plus $2.5M engineering and RETIE dossier |
| Ley 1715 / 2099 | IVA excluded | 19% VAT exclusion; 50% income-tax deduction shown separately |

Every one of these is an editable input in the UI. Cross-checked against
AutoSolar Colombia, OPS Colombia and Sunny Future: residential systems run
$3.5–5.5M COP per kWp, and a 5 kWp array with 5–10 kWh of LiFePO₄ is quoted at
$52–88M COP — which is where the default configuration lands.

Timeline figures reflect Colombian practice: 1–3 days of physical installation,
with operador de red interconnection paperwork taking 3–6 weeks and running in
parallel with procurement.

## Caveats

This is a planning-grade estimate, not a quotation. Before committing spend,
confirm roof area and orientation, panel-level shading, service entrance
capacity, and the operador de red's current interconnection rules.
