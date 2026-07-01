# LoL Nerf Predictor — League of Legends Champion Balance Forecasting

This repository contains a machine learning project designed to predict upcoming champion nerfs in the game League of Legends. The model analyzes statistical trends across 23 consecutive game patches to forecast which champions are highly likely to be tuned down by the balance team in the subsequent patch.

*Note: The source code, internal data processing logs, and the final presentation within this repository are written in Polish.*

---

## 💡 Project Overview & Methodology

The project models the game's balancing cycle as a time-series classification problem by leveraging **lagged features** (looking back at 5 preceding patches). 

### Key Features of the Pipeline:
* **Data Prep & Leakage Prevention:** Carefully handles time-dependent data. The "Trend" metric is safely engineered and shifted to prevent future data leakage during model training.
* **Feature Engineering:** Automated extraction of historical performance attributes per champion (`Score`, `Tier`, `Win%`, `Role%`, `Pick%`, `Ban%`, `KDA`) spanning across 5 patches.
* **Algorithmic Adaptations:** Utilizes the **C5.0 Decision Tree** algorithm. The script includes advanced data-cleaning workarounds specifically tailored for C5.0's core limitations (handling special characters like `%` or `_`, handling numeric column suffixes, and fixing unmapped empty factor levels by remapping them to `"missing"`).

---

## 📈 Model Evolution & Optimization

To achieve optimal predictive performance, the model went through three core stages of iterative improvement:

1. **Baseline C5.0 Tree:** Initial classification tree.
2. **Hyperparameter Tuning (Boosting):** Automated loops testing iterative trial sizes (`trials = 1:25`) which determined that **5 trials** yielded the highest True Positive rates.
3. **Cost-Sensitive Learning (Asymmetric Loss):** Since missing a nerf (Type II error) is more crucial than a false alarm, a custom cost matrix was implemented. Shifting the cost penalty up to `10` successfully boosted the true-positive predictive power to **over 50% accuracy** for patch 22 predictions.

---

## 📁 Repository Structure

* `lol_nerf_prediction.R` — Main R script containing data cleansing, feature engineering, looping mechanisms, model training, and cross-table validation.
* `*.csv` — Sequential patch data files containing champion statistics used as input dataframes.

---

## 🛠️ Built With

* **R** (v4.x recommended)
* **readr** & **dplyr** — Data manipulation and piping
* **C50** — Decision Trees and Rule-Based Models
* **gmodels** — Advanced model evaluation (`CrossTable`)

---

## 🚀 How to Run

1. Clone or download this repository to your local machine.
2. Place all the patch `.csv` files into the **same folder** where the `lol_nerf_prediction.R` script is located.
3. Open **RStudio** and open the script file.
4. Set your working directory to the script's location via the RStudio menu:
   `Session -> Set Working Directory -> To Source File Location`
5. Run the script. Ensure you have the required packages installed by running:
   ```R
   install.packages(c("readr", "dplyr", "C50", "gmodels"))
