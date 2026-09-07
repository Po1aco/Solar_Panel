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
- **Inverter capacity** in parallel-capable 8 kW hybrid units, checked against
  worst-case compressor surge
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
one 8 kW inverter, ≈$52.8M COP, ≈5.6 weeks**. The final form is about 21 panels,
35.8 kWh and 16 kW of inverter at ≈$111M COP.

## Model assumptions

| Parameter | Default | Basis |
|---|---|---|
| Peak sun hours | 5.5 kWh/m²·day | Santa Marta, IDEAM/UPME solar atlas — Colombia's highest band |
| Performance ratio | 78% | Soiling, cell temperature, battery round-trip |
| Depth of discharge | 90% | LiFePO₄ |
| Panel price | 889 COP/Wp | ≈$520k for a 585 Wp module (market $400k–750k) |
| Battery module | $6.5M COP | 5.12 kWh, 48 V, 100 Ah rack mount |
| Hybrid inverter | $9.5M COP | 8 kW, 48 V, parallel-capable, dual MPPT |
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
