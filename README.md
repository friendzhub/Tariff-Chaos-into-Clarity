# Tariff-Chaos-into-Clarity

# 📄 Mobile Supply‑Chain Tariff & Lead‑Time Simulator (Excel)

> **File:** `mobile_supplychain_sim_v7.xlsx`
>
> **Author:** Arun Kumar · 2025‑07‑13
>
> **Purpose:** Stress‑test a smartphone bill‑of‑materials (BOM) against swings in import duties, part costs, supplier lead‑times and logistics delays – all in a single, formula‑only Excel model.

---

## 1. What’s Inside the Workbook

| Sheet           | What you’ll see                                                                                                                                                                                      | Why it matters                                                                             |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Components**  | Baseline BOM – 10 key parts with *Base Cost*, *Tariff Rate (%)* and *Lead Time (days)*.                                                                                                              | Sets the deterministic starting point; edit yellow cells to try new baselines.             |
| **Assumptions** | Global levers such as `MarkupPct`, `GSTPct`, demand, and volatility (%) for cost, tariff and lead‑time draws.                                                                                        | Change these yellow cells once; they feed the entire model.                                |
| **Simulation**  | 1 000 Monte‑Carlo rows. Each row randomly samples cost, tariff & lead‑time for every component, then calculates:<br> `TotMfgCost`, `LongestLead`, `FulfillLead`, `ConsumerPrice`, `PipelineInvCost`. | Gives a probability distribution instead of a single scenario; press **F9** to regenerate. |
| **Summary**     | Mean, Std Dev, P5, P95 for the four key outputs.                                                                                                                                                     | Quick risk band for finance & ops.                                                         |
| **Sensitivity** | Pearson correlations between each component’s tariff and the simulated `ConsumerPrice`, plus a Tornado chart.                                                                                        | Pin‑points which duty line moves the consumer price the most.                              |

---

## 2. Quick Start

1. **Open** the workbook in Excel 2016 or later (no macros).
2. Ensure *Calculation Options* = **Automatic** (Formulas ▶ Calculation Options).
3. Go to **Assumptions** and tweak any yellow cell (e.g.

   * `TariffSwingPct` → `0.10` to allow ±10 pp duty shocks.
   * `CostSDPct` → `0` if you want tariffs‑only scenarios.
4. Press **F9** (or just watch – Excel auto‑recalc).
5. Check **Summary** & **Sensitivity** for updated risk stats and the Tornado chart.

---

## 3. How the Monte‑Carlo Engine Works

| Variable      | Distribution                                                               | Excel formula snippet                                                       |
| ------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **Cost**      | Normal, μ = Base Cost, σ = Base Cost × `CostSDPct`                         | `=BaseCost*(1+NORM.INV(RAND(),0,CostSDPct))`                                |
| **Tariff**    | Uniform (baseline ± `TariffSwingPct`)                                      | `=BaselineTariff + (RAND()*2-1)*TariffSwingPct`                             |
| **Lead Time** | Log‑Normal, CV = `LeadCV`                                                  | `=LOGNORM.INV(RAND(),LN(BaseLead)-0.5*LN(1+LeadCV^2),SQRT(LN(1+LeadCV^2)))` |
| **Demand**    | Deterministic – `MonthlyDemand` (can be made stochastic by adding a draw). | –                                                                           |

`FulfillLead = LongestLead + AssemblyDays + DistrDays`
`PipelineInvCost = (MonthlyDemand/30) × TotMfgCost × FulfillLead`

---

## 4. Customising Scenarios

* **Change baseline duties** – edit *Tariff Rate (%)* in **Components**.
* **Model a Free‑Trade Agreement** – set selected tariff baselines to `0` and reduce `TariffSwingPct`.
* **Simulate Port Congestion** – increase `DistrDays` from `5` → `12` on **Assumptions**.
* **Cost‑down Roadmap** – reduce *Base Cost* values after localisation initiatives.
* **Higher demand volatility** – (advanced) add a stochastic demand column and link `PipelineInvCost`.

---

## 5. Reading the Sensitivity Tornado Chart

* **Bars sorted by magnitude** – longer = stronger impact on consumer price.
* Correlation ≈ 0.25 → \~6 % of price variance explained (ρ²).
* Use it to prioritise lobbying or localisation efforts.

---

## 6. Version History

| Version | Date        | Key additions                                        |
| ------- | ----------- | ---------------------------------------------------- |
| v1–v3   | July 2025   | Base BOM, deterministic cost & lead model.           |
| **v4**  | 13 Jul 2025 | Added Monte‑Carlo, pipeline inventory calc.          |
| **v5**  | 13 Jul 2025 | All assumptions parameterised; demand deterministic.Corrected PipelineInvCost link to demand,Added Sensitivity sheet & Tornado chart |

---

## 7. License & Attribution

Feel free to **copy, fork, or extend** this model under the MIT License.
If you build on it, a tag back to this LinkedIn post would be appreciated! 🙏

---

## 8. Contact

LinkedIn DM:[* **Arun Kumar**](https://www.linkedin.com/in/arun-k-768b5b28/)
Email:* arun_kumar_pgppro2025@isb.edu .
Always keen to swap supply‑chain modelling tips!*
