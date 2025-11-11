# BMI500 — Week 11 Homework

**Author:** Hanqi Chen  
**Email:** [hchen54@emory.edu](mailto:hchen54@emory.edu)  
**Selected Question:** HW 1: SIR and SEIR Model Implementation for Pandemic Spread<br>
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
  ![B](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/b.png)
  Single wave. **I(t)** rises fast and then decays. No exposed stage and no μ, so once susceptibles drop and recoveries win, the curve does not come back.  
  *Plot:* see **HW11.ipynb → Part B SIR plot**  

- **SEIR (β=0.0003, γ=0.1, σ=0.2, μ=0.01, S0=990, E0=9, I0=1, R0=0; 0–365 days):** 
  ![Dii_365](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/Dii_365.png) 
  First big peak, then a smaller bump. The **E→I delay (σ)** pushes the peak **later** than SIR. With **μ>0**, births add back **S**, so a second rise appears.  
  *Plot:* see **HW11.ipynb → D(ii) 365-day SEIR plot**  

- **SEIR long run (same params; 0–1200 days):**  
  ![Dii_1200](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/Dii_1200.png) 
  Damped oscillations. Peaks get smaller and settle to a low level (endemic-like). This behavior does not show up in the 150-day SIR run because SIR has no E and no μ.  
  *Plot:* see **HW11.ipynb → D(ii) 1200-day SEIR plot**  

- **Sensitivity (E(i) lines & E(ii) 20-bar charts):**
  ![ei_beta](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/Ei_beta.png) 
  ![ei_gamma](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/Ei_gamma.png) 
  ![eiii_peak](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/eiii_peak.png) 
  ![eiii_total](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/eiii_total.png)  
  Increasing **β** makes peaks **higher** and **earlier**; increasing **γ** makes peaks **lower** (and typically earlier) and reduces **total infections**. This trend is consistent in both models, but the **magnitude** and **timing** differ because SEIR has the latent stage (σ) and demographics (μ).  
  *Plots:* see **HW11.ipynb → E(i) β-sweep & γ-sweep** and **E(ii) 20-column bar plots (peak + total)**  

**Difference:** SIR (no E, μ=0) gives a single, earlier peak over a short horizon (0–150 d). SEIR (with E and μ) shifts the peak later (σ delay) and can show extra waves (μ replenishes S). Over a full year and beyond, SEIR matches the “spread then settle” pattern seen in the long-horizon plots.

---

## Relevance to Model-Based Machine Learning

This work connects simple epidemic models with ideas in model-based machine learning. Both SIR and SEIR describe how the system changes based on a few clear parameters — **β**, **γ**, **σ**, and **μ**. These parameters are not hidden weights like in black-box ML models but have direct meaning: infection rate, recovery rate, incubation rate, and population change.  

In model-based ML, we often start from a known rule and then adjust parameters to match data. Here, the ODE models already give the rule (how S, E, I, R interact), and changing the parameters is like learning how the system behaves under different conditions.  
The **RK45 solver** runs this rule step by step, similar to how a model-based agent would simulate future states.  

The parameter sensitivity we did in **E(i–ii)** shows the same idea as interpretability in ML — we can see exactly how changing β or γ changes the output curve. That makes the model explainable and closer to how model-based ML tries to use structure instead of pure data fitting.  

For improvement, future work could make β or γ change with time or policy (β(t), γ(t)) and fit them with data. That would let the system learn from data but still keep the clear structure of the model.

---

## Suggestions for Future Modeling Improvements 

![nov_beta](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/nov_beta.png) 
![nov_365](https://github.com/Hanqk97/BMI500_WEEK11/blob/main/plots/nov_365.png)

Since the beta in above model is constant, but during the whole year the social distance may changed fro perople due to the season change will change the temperal that may increase or decresat their distance, to further close to ther real world result, do a ***novleyt*** test that add a only the transmission rate was changed to a seasonal form, β(t) = β · (1 + 0.2·sin(2πt/365)), and everything else stayed the same as in the question. The result shows a higher and slightly earlier first peak with seasonal β (I_max = 206.86 at t = 57.0 d) compared to the constant-β run (I_max = 179.46 at t = 60.3 d), while the yearly total infections became smaller with seasonality (2588.20 vs 2673.54). The plots match this story: early in the year β(t) is above baseline so the first surge is stronger, and later β(t) drops below baseline so the tail is weaker.

To further improve the model, a simple next step would be adding a **vaccination flow** from S to R with a small constant rate v. This addition would show how continuous immunization lowers both the peak and total infections while keeping the model structure almost unchanged.  
Another easy extension would be including a **policy change on β**, where β decreases suddenly at a given day to represent the start of interventions like lockdowns or mask mandates. This case can be compared to the baseline to show how timing and strength of policy affect epidemic size.  
Finally, adding a small **random variation on β(t)** could represent uncertainty in contact behavior and create a visible spread of outcomes, closer to real-world data.  
These extensions would make the model more flexible and realistic while staying simple enough for clear explanation and grading.

---

## AI Tool Disclosure

Generative AI (ChatGPT) was used to complete parts **A** and **E(iii)**.

**Prompts used:**

1. **Prompt 1:** “Which ODE solver (Euler or Runge–Kutta) gives better stability and accuracy for SIR equations, and can you show how to implement it in Python?”  
   **Result:** attached in `ai_disclosure/prompt1.pdf`, or view the share link:  
   https://chatgpt.com/share/6912b58b-8f3c-8001-8fbc-a29a9404831e

2. **Prompt 2:** “How to generate a colorful Matplotlib bar plot (20 columns) with labeled values on top for each (β, γ) pair from the SEIR model?”  
   **Result:** attached in `ai_disclosure/prompt2.pdf`, or view the share link:  
   https://chatgpt.com/share/6912bee9-0f40-8001-a112-3fbc2664f230
---
