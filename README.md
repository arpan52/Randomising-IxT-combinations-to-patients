# Randomising-IxT-Combinations-to-Patients

This repository contain the source R codes (.qmd files) for all simulations conducted in the paper **_Orthogonal factorial designs for trials of therapist-delivered interventions: Randomising intervention–therapist combinations to patients_.** It also have R datasets (.rds files) that are used in Supplementary File F of the paper.

---

## 📘 Description of both the codes

The codebase **Final_qmd_Code_Parameter_Estimates.qmd** and **Final_qmd_Code_Parameter_Estimates.qmd** includes functions corresponding to five randomisation scenarios as described in **Section 5** of the paper. Each function simulates different randomisation strategies of patients to interventions and therapists.

---

## 🔧 Inputs

To run a simulation, the user must provide the following inputs:

- `Design_1`: Integer (1–5) — Specifies the first randomisation method  
- `Example_1`: Integer (1–3) — Specifies the example scenario under `Design_1`  
- `Design_2`: Integer (1–5) — Specifies the second randomisation method  
- `Example_2`: Integer (1–3) — Specifies the example scenario under `Design_2`  
- `del_0`: Numeric scalar — Overall mean effect ($$\delta_0$$)  
- `del_1`: Numeric scalar — Difference between the intervention mean and overall mean ($$\delta_1$$)  
- `del_2`: Numeric 2 by 2 vector — Patient baseline severity ($$\delta_2$$)   
- `del_3`: Numeric scalar — Patient baseline age (($$\delta_3$$)   
- `n_sim`: Integer — Number of simulations (default: `10000`)

---


---

## 💡 Inputs of *Final_qmd_Code_Parameter_Estimates.qmd*

```r
# Set inputs
# model parameters for final comparison
del_0<- 0               # delta_0 =  The overall mean effect
del_1<- 0.315          # delta_1 = The difference between the mean of all outcomes for the intervention
                       #           and the overall mean delta_0. 
                      #  del_1<- 0.315   For Example 1,2
                      #  del_1<- 0.180   For Example 3
del_2<- c(0,0.2)     # delta_2 is the measure of patient severity at the baseline
del_3<- 0           # delta_3 is the measure of patient Age at the baseline
n_sim <- 10          # Number of simulations

# First Design/Randomisation method and Example to compare
Design_1<-1
Example_1<-3

# Second Design/Randomisation method and Example to compare to the first one
Design_2<-5
Example_2<-2

# Staring the simulations for Design_1 and Example_1
results_1<-Comarison_D_ij(Design_1,Example_1,del_0,del_1,del_2,del_3,n_sim)
# Staring the simulations for Design_2 and Example_2
results_2<-Comarison_D_ij(Design_2,Example_2,del_0,del_1,del_2,del_3,n_sim) 


C <- cbind(results_1, results_2)              # put them side by side
idx <- as.vector(rbind(1:ncol(results_1), ncol(results_1) + 1:ncol(results_2)))
C_interleaved <- C[, idx]
# custom colomn names
custom_names <- c(
  paste("delta_1 D", Design_1, Example_1),
  paste("delta_1 D", Design_2, Example_2),
  paste("SE of delta_1 D", Design_1,Example_1),
   paste("SE of delta_1 D", Design_2,Example_2),
 paste("Therapist Effect D",Design_1, Example_1),
   paste("Therapist Effect D",Design_2, Example_2),
paste("Intervention X Therapist effect D", Design_1, Example_1),
  paste("Intervention X Therapist effect D", Design_2, Example_2),
paste("Type-2 error D", Design_1, Example_1),
paste("Type-2 error D", Design_2, Example_2)
)

# Colomn and row names
colnames(C_interleaved) <- rep(custom_names)
rownames(C_interleaved) <- c( paste("(del_3,del_2)=", del_3, del_2[1]),paste("(del_3,del_2)=", del_3, del_2[2]))

print("Final Results")
print(C_interleaved)  # Result
```


---


---


## 📤Reading Output *Final_qmd_Code_Parameter_Estimates.qmd*

The output is a **2 × 10 matrix** comparing the performance of the two specified designs.

Each row corresponds to one `(del_3, del_2)` pair:

* **Row 1** → `(del_3, del_2) = (0, 0)`
* **Row 2** → `(del_3, del_2) = (0, 0.2)`

Let **i** denote the **Design / Randomisation method** and **j** denote the **Example**, then each row contains the following metrics:

| Metric | Description |
| --- | --- |
| delta_1 D i j | Estimated treatment contrast for Design i, Example j |
| SE of delta_1 D i j | Standard error of the contrast for Design i, Example j |
| Therapist Effect D i j | Std. deviation of between therapist random effects ($\sigma_{u_1}$) for Design i, Example j |
| Intervention X Therapist effect D i j | Std. deviation of IxT interaction effects ($\sigma_{v_1}$) for Design i, Example j |
| Type-2 error D i j | Type II error rate for Design i, Example j |

---

### Output

|  | delta_1 D13 | delta_1 D52 | SE of delta_1 D13 | SE of delta_1 D52 | Therapist Effect D13 | Therapist Effect D52 | Intervention X Therapist effect D13 | Intervention X Therapist effect D52 | Type-2 error D13 | Type-2 error D52 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| (del_3, del_2) = 0,0 | 0.290 | 0.320 | 0.141 | 0.118 | 0.102 | 0.074 | 0.098 | 0.143 | 0.6 | 0.4 |
| (del_3, del_2) = 0,0.2 | 0.343 | 0.331 | 0.147 | 0.142 | 0.049 | 0.115 | 0.089 | 0.046 | 0.4 | 0.6 |

---

## 💡 Inputs of *Final_qmd_Code_Type-1_error_and_Boundary_estimates.qmd*

```r
# Set inputs
# model parameters for final comparison
del_0<- 0               # delta_0 =  The overall mean effect
del_1<- 0.315          # delta_1 = The difference between the mean of all outcomes for the intervention
                       #           and the overall mean delta_0. 
                      #  del_1<- 0.315   For Example 1,2
                      #  del_1<- 0.180   For Example 3
del_2<- c(0,0.2)     # delta_2 is the measure of patient severity at the baseline
del_3<- 0           # delta_3 is the measure of patient Age at the baseline
n_sim <- 10          # Number of simulations

# First Design/Randomisation method and Example to compare
Design_1<-2
Example_1<-2

# Second Design/Randomisation method and Example to compare to the first one
Design_2<-4
Example_2<-1

# Staring the simulations for Design_1 and Example_1
results_1<-Comarison_D_ij(Design_1,Example_1,del_0,del_1,del_2,del_3,n_sim)
# Staring the simulations for Design_2 and Example_2
results_2<-Comarison_D_ij(Design_2,Example_2,del_0,del_1,del_2,del_3,n_sim) 


C <- cbind(results_1, results_2)              # put them side by side
idx <- as.vector(rbind(1:ncol(results_1), ncol(results_1) + 1:ncol(results_2)))
C_interleaved <- C[, idx]
# custom colomn names
custom_names <- c(
    paste("Type-1 error D", Design_1, Example_1),
  paste("Type-1 error D", Design_2, Example_2),
  paste("% Boundary Estimates D", Design_1,Example_1),
   paste("% Boundary Estimates D", Design_2,Example_2),

)

# Colomn and row names
colnames(C_interleaved) <- rep(custom_names, each = 2)
rownames(C_interleaved) <- c( paste("(del_3,del_2)=", del_3, del_2[1]),paste("(del_3,del_2)=", del_3, del_2[2]))

print("Final Results")
print(C_interleaved)  # Result



```
---

---
## 📤Reading Output *Final_qmd_Code_Type-1_error_and_Boundary_estimates.qmd*

The output is a **2 × 10 matrix** comparing the performance of the two specified designs.

Each row corresponds to one `(del_3, del_2)` pair:

* **Row 1** → `(del_3, del_2) = (0, 0)`
* **Row 2** → `(del_3, del_2) = (0, 0.2)`

Let **i** denote the **Design / Randomisation method** and **j** denote the **Example**, then each row contains the following metrics:

| Metric | Description |
| --- | --- |
| Type-1 error D i j | Type I error rate for Design i, Example j |
| % Boundary Estimates D i j | Percentage of boundary estimates out of n_sim estimates of Therapist and IxT interaction estimates for Design i, Example j |

---

### Output

|  | Type-1 error D22 | Type-1 error D41 |% Boundary Estimates D22 |% Boundary Estimates D41 |
| --- | --- | --- | --- | --- |
| (del_3, del_2) = 0,0 | 0.7 | 0.8 | 10 | 10 |
| (del_3, del_2) = 0,0.2 | 0.5 | 0.7 | 60 | 30| 

---
