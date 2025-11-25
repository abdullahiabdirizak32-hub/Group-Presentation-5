# Group-Presentation-5

Project notebooks, data and object-oriented analysis code for the term project: exploring the relationship between nitrogen fertilizer application and soil pH, plus downstream crop yield considerations.

## Team

- Albright Maduka
- Aishwarya Thazhath
- Abdullahi Mohamed

## Files of interest

- `Group Presentation 5.ipynb` — main team presentation notebook combining EDA, preprocessing, transformation, exploratory plotting, and model experiments. Includes object-oriented classes for processing and modeling.
- `Problem_Analysis_Workshop_4_classes.ipynb` — a refactor / workshop notebook where the core pipeline and modeling steps were encapsulated into classes and accompanied by short reflections after outputs.
- `Data/Crop_recommendationV2.csv` — dataset used by the notebooks.
- `requirements.txt` — pinned Python packages used in the environment. (See notes below about adding any missing packages.)

## Summary of what the notebooks contain

Both notebooks follow a similar analysis flow and share the same classes and helpers implemented in the workshop notebook. The pipeline runs full dataset ingestion → preprocessing → transformations → simple statistical modeling. Key steps and outputs are described briefly below:

1. Data loading and light EDA
	- Loads `Data/Crop_recommendationV2.csv` and prints a sample of rows and summary statistics.
	- Purpose: confirm schema, detect missing values, and inspect ranges and distributions.

2. Visual EDA
	- Scatterplots show relationships between `N` (nitrogen), `ph` (soil pH), and `yield` for quick visual inspection.

3. Data preprocessing (encapsulated in `DataPipeline` class)
	- `DataPipeline.load_data()` — loads the CSV into `self.df`.
	- `DataPipeline.basic_info()` / `describe()` — prints dataset info and numeric summaries.
	- `DataPipeline.encode_growth_stage()` — maps categorical growth stages to numeric codes.
	- `DataPipeline.date_to_julian()` — converts date column to a `julian_day` numeric feature.
	- `DataPipeline.dummify_label()` — one-hot encodes the `label` categorical column.
	- `DataPipeline.transform_n_boxcox()` — Box–Cox transform applied to the `N` column (requires positive values; transformer stored as `bc_transformer`).
	- `DataPipeline.transform_ph_yeojohnson()` — Yeo–Johnson transform applied to `ph` producing `ph_tl` (transformer stored in `yj_transformer`).

4. Modeling classes
	- `LinearRegressionModel` — a lightweight OO wrapper around scikit-learn's LinearRegression (methods: `fit`, `r2`, `plot_fit`, `residuals`, `plot_residuals_vs_fitted`). Used for baseline linear fits (example: `N_bc` → `ph_tl`).
	- `PolynomialRegressionModel` — combines sklearn's PolynomialFeatures + LinearRegression for degree‑controlled polynomial regression (methods: `fit`, `r2`, `plot_fit`). Used to explore nonlinearity.
	- `StatisticalModel` (in the workshop): an OLS/statsmodels wrapper for hypothesis/tests and summaries (fit via ordinary least squares when needed).

5. Diagnostics & interpretation
	- R² values for linear and polynomial fits are printed for easy comparison.
	- Residuals vs fitted plot used to visually inspect homoscedasticity.
	- Notebook cells include short 3–4 sentence reflections immediately following output-producing cells to explain what the output means and what to look for next.

## Notable findings in the notebook (high level)

- A simple linear model using transformed Nitrogen (`N_bc`) to predict transformed pH (`ph_tl`) explained a very small fraction of variance (R² ~ 0.014), indicating a weak linear relationship.
- A degree-2 polynomial produces a small improvement (R² slightly larger), while very high-degree fits (e.g., degree=10) increase R² but likely overfit.
- Residual checks (plots and tests) are included to evaluate model assumptions like homoscedasticity.

## How to run this project locally (Windows / PowerShell)

1. Create and activate a virtual environment. From the repository root run (PowerShell):

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
```

2. Install the pinned dependencies and add statsmodels (not pinned in `requirements.txt`) — statsmodels is required by some notebook cells:

```powershell
pip install -r requirements.txt
pip install statsmodels
```

3. Start Jupyter (recommended) or open the notebooks in VS Code and run cells sequentially:

```powershell
jupyter notebook
```

4. If you edit class definitions, either re-run the class-defining cell before using the class or restart kernel and run all cells to avoid in-memory mismatch errors.

## Notes and next steps (suggested)

- Add unit tests that instantiate `DataPipeline`, run transformations, and assert the expected columns exist — this makes the pipeline robust to changes.
- Consider extracting classes into a module (e.g. `pipeline.py` / `models.py`) for import into scripts and tests.
- Add cross-validation and hold-out evaluation when comparing linear vs. polynomial models to detect overfitting reliably.
- Optionally add a small `SUMMARY.md` or `presentation_slides/` folder that extracts key figures and numbers used in the team presentation.

## Contact / contributions

If you'd like changes or additional automated tests and extraction of the classes to a Python package/module, raise an issue or request changes in the repo.

---
Updated to include detailed notebook descriptions and usage instructions.