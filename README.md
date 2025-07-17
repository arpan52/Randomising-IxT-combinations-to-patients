# Randomising-IxT-Combinations-to-Patients

This repository contains the source R code for all simulations conducted in the paper:

**_Orthogonal factorial designs for trials of therapist-delivered interventions: Randomising intervention–therapist combinations to patients_**

---

## 📘 Description

The codebase includes functions corresponding to five randomisation scenarios as described in **Section 5** of the paper. Each function simulates the randomisation of patients to combinations of interventions and therapists (IxT).

---

## 🔧 Inputs

To run a simulation, the user must provide the following inputs:

- `Design_1`: Integer (1–5) — Specifies the first randomisation design  
- `Example_1`: Integer (1–3) — Specifies the example scenario under `Design_1`  
- `Design_2`: Integer (1–5) — Specifies the second randomisation design  
- `Example_2`: Integer (1–3) — Specifies the example scenario under `Design_2`  
- `del_0`: Numeric scalar — Overall mean effect (δ₀)  
- `del_1`: Numeric scalar — Difference between the intervention mean and overall mean (δ₁)  
- `del_2`: Numeric vector — Patient baseline severity (δ₂)  
- `del_3`: Numeric scalar — Patient baseline age (δ₃)  
- `n_sim`: Integer — Number of simulations (default: `10000`)

---

## 📤 Output

The output is a **2 × 7 matrix** comparing the performance of the two specified designs:

Each row corresponds to one (Design, Example) pair:
- **Row 1** → `(Design_1, Example_1)`
- **Row 2** → `(Design_2, Example_2)`

Each row contains the following metrics:

| Metric | Description |
|--------|-------------|
| Contrast Estimate | Estimated treatment contrast |
| Standard Error    | Standard error of the contrast |
| SD (Therapist)    | Std. deviation of therapist random effects |
| SD (IxT Combo)    | Std. deviation of IxT interaction effects |
| Type I Error      | Type I error rate |
| Type II Error     | Type II error rate |
| Proportion Singular | Proportion of singular model fits |

---

## 💡 Example Usage

```r
# Set inputs
Design_1 <- 2
Example_1 <- 1
Design_2 <- 4
Example_2 <- 2
del_0 <- 0
del_1 <- 1
del_2 <- c(0, 0.2)
del_3 <- 0.5
n_sim <- 10000

# Run simulation
results_1<-Comarison_D_ij(Design_1,Example_1,del_0,del_1,del_2,del_3,n_sim) 
results_2<-Comarison_D_ij(Design_2,Example_2,del_0,del_1,del_2,del_3,n_sim) 

# View Results
print(rbind(results_1,results_2))


