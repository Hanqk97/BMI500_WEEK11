# BMI500 — Week 11 Homework

**Author:** Hanqi Chen  
**Email:** [hchen54@emory.edu](mailto:hchen54@emory.edu)  
**Selected Question:** HW 1: SIR and SEIR Model Implementation for Pandemic Spread 
**Repository:** [BMI500_WEEK11](https://github.com/Hanqk97/BMI500_WEEK11)  
**Notebook:** [HW11.ipynb](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/HW11.ipynb)

---

## Repository Summary

All **code, plots, and written answers for every part (A → E)** are contained in **`HW11.ipynb`** for easy review — no separate report file.

The notebook includes:
- Full implementation of the SIR and SEIR models.  
- Step-by-step simulations (A–D).  
- Parameter sensitivity analyses (E i-ii).  
- Policy interpretation (E iii).  
- All plots, results, and explanatory text in Markdown cells.

---

## Key Insights

1. **Peak Infection Logic (C):**  
   The infection peak occurs when `β·S = γ`. With β = 0.0003 and γ = 0.1, the theoretical S* ≈ 333.33 matched the simulation (S ≈ 332), confirming correctness of the ODE system.

2. **Parameter Sensitivity (E i):**  
   - Increasing **β** raises and advances the infection peak.  
   - Increasing **γ** lowers and advances the peak.  
   These behaviors match the expected dynamics of the SEIR model.

3. **Peak / Total Infection Comparison (E ii):**  
   - Combined β–γ bar plots show that higher **β** → larger peaks & totals;  
     higher **γ** → smaller peaks & totals.  
   - When β is large and γ is small, outbreaks expand sharply;  
     when β is small or γ is large, the spread remains limited.

4. **Policy Interpretation (E iii):**  
   - Reducing **β** (social distancing, masks, vaccines) and raising **γ** (early testing, better care) both lower peaks and reduce total infections.  
   - Combined β↓ + γ↑ gives the smallest outbreak and fastest containment.

---

## Comparative Model Performance

- **SIR (β=0.0003, γ=0.1, S0=999, I0=1, R0=0; 0–150 days):**  
  Single wave. **I(t)** rises fast and then decays. No exposed stage and no μ, so once susceptibles drop and recoveries win, the curve does not come back.  
  *Plot:* see **HW11.ipynb → Part B SIR plot**  

- **SEIR (β=0.0003, γ=0.1, σ=0.2, μ=0.01, S0=990, E0=9, I0=1, R0=0; 0–365 days):**  
  First big peak, then a smaller bump. The **E→I delay (σ)** pushes the peak **later** than SIR. With **μ>0**, births add back **S**, so a second rise appears.  
  *Plot:* see **HW11.ipynb → D(ii) 365-day SEIR plot**  

- **SEIR long run (same params; 0–1200 days):**  
  Damped oscillations. Peaks get smaller and settle to a low level (endemic-like). This behavior does not show up in the 150-day SIR run because SIR has no E and no μ.  
  *Plot:* see **HW11.ipynb → D(ii) 1200-day SEIR plot**  

- **Sensitivity (E(i) lines & E(ii) 20-bar charts):**  
  Increasing **β** makes peaks **higher** and **earlier**; increasing **γ** makes peaks **lower** (and typically earlier) and reduces **total infections**. This trend is consistent in both models, but the **magnitude** and **timing** differ because SEIR has the latent stage (σ) and demographics (μ).  
  *Plots:* see **HW11.ipynb → E(i) β-sweep & γ-sweep** and **E(ii) 20-column bar plots (peak + total)**  

**Difference:** SIR (no E, μ=0) gives a single, earlier peak over a short horizon (0–150 d). SEIR (with E and μ) shifts the peak later (σ delay) and can show extra waves (μ replenishes S). Over a full year and beyond, SEIR matches the “spread then settle” pattern seen in the long-horizon plots.
---

## Relevance to Model-Based Machine Learning

This exercise shows how **mechanistic modeling** (SIR/SEIR differential equations) and **data-driven parameter tuning** connect to model-based ML:

- The ODE parameters (β, γ, σ, μ) are interpretable **model parameters** rather than black-box weights.  
- Simulation results can serve as **priors or constraints** for hybrid ML systems (e.g., Bayesian epidemic forecasting, RL for intervention policies).  
- Sensitivity analysis demonstrates **explainable parameter influence**, aligning with ML interpretability goals.

---

## Suggestions for Future Modeling Improvements (Novelty Ideas)

To extend novelty beyond the baseline assignment:
- **Parameter Estimation via ML:** Fit β, γ, σ, μ using gradient-based or probabilistic learning from real case data.  
- **Uncertainty Modeling:** Add stochastic noise to represent reporting delays or random contacts.  
- **Adaptive β(t):** Model β as a function of mobility or policy strength, learnable through regression or RL.  
- **Comparative Learning:** Train a simple neural network on synthetic SEIR data to approximate I(t) and compare predictive generalization.

Each of these would connect the classical model with data-driven adaptation — a core concept in model-based ML.

---

## AI Tool Disclosure

Generative AI (ChatGPT) was used **only** for specific technical assistance, not for producing final numerical outputs.

**Prompts used:**

1. Prompt 1: “Which ODE solver (Euler or Runge–Kutta) gives better stability and accuracy for SIR equations, and can you show how to implement it in Python?”  


2. Prompt 2: “How to generate a colorful Matplotlib bar plot (20 columns) with labeled values on top for each (β, γ) pair.”  
   *(Result: used `plt.cm.tab20` color palette and value annotations above each bar.)*

All code and explanations were verified, rewritten, and executed manually in the notebook.

**Disclaimer:**  
> Generative AI (ChatGPT) was used to assist in code formatting, visualization, and solver selection discussion.  
> All model logic, parameter tuning, execution, and interpretation were conducted independently by Hanqi Chen.

---

## Environment Setup

### 1. Requirements
Create a file named **`requirements.txt`** in your environment: