# Session memory — Income shocks & household asset allocation review

> Resume file for the literature review. Read this first when picking the project
> back up (especially in a new web session). Last updated: 2026-06-01.

## Goal

Literature review of studies that **causally** estimate how households that
suddenly get richer change **(I) total saving** and **(II) the allocation of
saving across asset classes** (≥ financial-vs-real split). Canonical design: an
exogenous windfall (cash-transfer RCT, asset grant, lottery) with the household
balance sheet observed before/after. Need it across the **development spectrum**
(very poor → rich), with **baseline income/wealth/consumption** for treated and
control, and treatment effects in a **standardized unit**.

## Decisions locked in (confirmed with user)

- **Primary standardized unit = MPX** = Δasset_X / windfall (dimensionless, 0–1;
  sums to ≈1 across consumption + all asset classes + debt paydown). Secondary
  columns: USD-PPP (base year **2016**), % of control mean, SD units.
- **Recording principle:** atomic extraction row = the **most granular asset line
  item the paper reports** (livestock, roof, land, gold, farm equipment, deposits,
  equity, ROSCA, M-Pesa, …). Financial/real are **roll-ups computed from** those,
  never the recording granularity. `asset_subclass` = paper's verbatim label;
  `asset_class` ∈ {financial, real, debt, aggregate, consumption_flow}.
- **Output format = LaTeX synthesis note + machine-readable CSV** (repo convention).
- **Dev-spectrum target:** ≥2 studies per GDP-pc-PPP band: <$3k, $3–12k,
  $12–30k, >$30k.
- Gated papers wait for **Columbia library full-text access** (not yet set up).

## Files in repo (branch `claude/income-shocks-asset-allocation-feuM4`)

- `income_shocks_asset_allocation.tex` — synthesis note (plan, taxonomy, MPX
  method, worked extraction, pipeline). Compiles with `pdflatex`+`bibtex`
  (no LaTeX toolchain in the web container, so no PDF committed yet).
- `extraction.csv` — long format, one row per paper × asset line item. 20 cols:
  `paper_id, authors, year, country, gdp_pc_ppp_band, identification, shock_type,
  shock_usd_ppp_2016, horizon, baseline_cons_usd_ppp_mo, baseline_assets_usd_ppp,
  asset_class, asset_subclass, te_usd_ppp_2016, control_mean_usd_ppp, mpx,
  te_pct_control, te_sd, source_table, status`. `status` ∈ {verified, gated}.
- `references.bib` — 13 seed entries appended (keys below).
- Commit so far: `10f6d54` "Add income-shocks/asset-allocation review plan and
  first extraction".

## Done so far — 3 ungated seed papers (verified cells)

- **Haushofer & Shapiro 2016 QJE — Kenya (<$3k).** UCT RCT, ~9mo horizon.
  Transfers $404 / $1,525 PPP (avg ≈$709). Control monthly cons $158 PPP,
  TE +$36. Home/iron-roof +$359 PPP (+60%); net non-land movable assets +$177
  PPP (+26%); M-Pesa savings +$3 PPP. MPX_real ≈0.28 (housing 0.19, movable
  0.09); MPX_financial ≈0.
- **Briggs, Cesarini, Lindqvist & Östling 2021 JFE — Sweden (>$30k).** Lottery.
  $150k windfall → stock-market participation +12pp for prior non-participants,
  ~0 for existing holders. Financial/equity extensive margin.
- **Fagereng, Holm & Natvik 2021 AEJ:Macro — Norway (>$30k).** Lottery
  ($1.5k–150k). First-year MPC ≈0.5 (range 0.4–1.0); marginal propensity to save
  ≈0.5, buffered first into deposits.

**Organizing contrast (the review's spine):** poor households → real/productive
assets (livestock, durables, roofs); rich households → financial assets
(deposits, then equity).

## BLOCKER (why we stopped)

This web session's **network policy** allows only `WebSearch`. `WebFetch` is
**blocked for every domain** (even Wikipedia 403s) and direct `curl` hits a host
**allowlist** ("Host not in allowlist"). So no URL — even the ungated Haushofer
PDF at `https://haushofer.ne.su.se/publications/Haushofer_Shapiro_UCT_QJE_2016.pdf`
— can be fetched. No `pdftotext`/python PDF lib and `pip install` is blocked too.
BUT: the `Read` tool reads PDFs natively, so **a PDF committed to the repo can be
read locally**.

### To unblock (next session)
1. **Preferred:** start the new web session with a **more permissive network
   policy** (allow NBER, SSB, university repos, journal/publisher domains). See
   network-policy docs: https://code.claude.com/docs/en/claude-code-on-the-web
2. Or **commit the PDFs** to the branch → read them with the `Read` tool.
3. Or **paste the relevant tables** into chat for immediate standardization.

## Next steps (full execution)

1. Fill Haushofer & Shapiro `gated` cells to full granularity (livestock,
   durables, ag assets, business, land, savings sub-items) — QJE assets table.
2. Fill Norway deposit/equity/debt split (Fagereng et al.) and Sweden risky-share
   intensive margin (Briggs et al.).
3. Extend across the four dev bands using the pipeline below.
4. Complete USD-PPP-2016 conversions + baseline columns for every row.
5. Verify within-paper MPX (consumption + assets + debt) ≈ 1.
6. Expand the .tex comparison table + development-gradient discussion; compile.

## Candidate pipeline (bib keys)

- Subsistence <$3k: `haushofer2016short` (done), `egger2022general` (Kenya GE),
  `blattman2014generating` / `blattman2020long` (Uganda grants),
  `banerjee2015multifaceted` (6-country graduation), `demel2008returns` (Sri Lanka).
- Lower/upper-middle $3–30k: `gertler2012investing` (Mexico Oportunidades);
  add Brazil Bolsa Família.
- Rich >$30k: `fagereng2021mpc` (done), `fagereng2019saving` (Norway saving across
  wealth dist.), `briggs2021windfall` (done), `cesarini2017effect` (Sweden),
  `imbens2001estimating` (US lottery), `golosov2024lottery` (US lottery).

## Open spec caveat

Haushofer & Shapiro "total non-land assets" effect appears as +$177 (+26%),
+$164 (+23%), and +$301 (+61%) across sources (endline round × saturation
sub-sample). Used the most-cited headline; pin the exact table/column from the
PDF in the full pass.
