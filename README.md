## Data-driven Design of Pseudohalide Perovskites for Optoelectronic Applications
## Overview
This repository contains Jupyter notebooks for data acquisition, preprocessing, exploratory data analysis and developing machine learning (ML) models to predict bandgaps in pseudohalide perovskites (e.g., ABX<sub>3</sub>  where X = SCN<sup>–</sup> , CN<sup>–</sup> , N<sub>3</sub><sup>–</sup> , BF<sub>4</sub><sup>–</sup>, and ClO<sup>–</sup> ). 

**Data Sources**: 

- [Hybrid Organic-Inorganic Perovskites (HOIP)](https://www.nature.com/articles/sdata201757)
- [Shannon Ionic Radii](http://abulafia.mt.ic.ac.uk/shannon/ptable.php) 
- [Materials Project (MP) queries for inorganic analogs](https://next-gen.materialsproject.org)
- [Inorganic Crystal Structure Database (ICSD)](https://icsd.products.fiz-karlsruhe.de/)
- Open Quantum Materials Database (OQMD)

**Feature Engineering**: 

* **Feature descriptors** 

- **Goldschmidt Tolerance Factor** $(t)$  
  Measures the geometric stability of the perovskite structure:  
  
  $
  t = \frac{r_A + r_X}{\sqrt{2}(r_B + r_X)}
  $ 
   
  Typical stable cubic perovskite range: 
  
  $0.9 \leq t \leq 1.1$

- **Octahedral Factor** $(\mu)$  
  Describes the suitability of the B-site cation to form a stable [BX₆] octahedron:  
  
  $
  \mu = \frac{r_B}{r_X}
  $  
  
  Empirically stable range: $\mu > 0.414$

where:  

- $r_A$: effective ionic radius of the A-site cation
- $r_B$: ionic radius of the B-site cation
- $r_X$: ionic radius of the anion

* **ionic character**
* **volume**
* **density** 
* **electronegativity differences**
* **dipole moments** 

	These capture perovskite stability, lattice distortion, and electronic effects critical for bandgap.
* **Synthetic samples**: Synthetic data generation uses ionic radii (e.g., SCN⁻ ~2.13 Å) and empirical adjustments to simulate bandgap shifts due to larger anions, which weaken B-X bonds and narrow bandgaps.
* **ML Pipeline**: Data preprocessing, feature engineering, standalone models including Extreme Gradient Boosting (XGBoost), Random Forest (RF), Gradient Boosting Regressor (GBR) and Neural Networks(NN), ensemble modeling (XGBoost + NN), uncertainty estimation via quantile regression, and SHAP for interpretability.
* **Alignment**: Features align with perovskite physics (e.g., tolerance factor >0.8 for cubic stability; pseudohalides reduce toxicity vs. Pb-halides while tuning optoelectronics).

The model achieves ~0.1-0.4 eV MAE on test sets, suitable for screening lead-free perovskites for solar cells.



## Requirements
- Python 3.10+
- Materials Project API key (set as `MP_API_KEY` environment variable).
- Dependencies: See `requirements.txt`.

## Setup
1. Clone the repo: `git clone <repo-url>`
2. Install dependencies: `pip install -r requirements.txt`
3. Download HOIP CIF files into `data/HOIP_cif_extracted/` (source from literature or databases).
4. Notebooks: 
	- `datacrawler.ipynb` contains data used from published articles.
	- `DataEDA.ipynb` contains exploratory data analysis on the different data sources.
	- `FormationEnergy.ipynb` contains information on Formation Energy Prediction.
	- `Modeling.ipynb` contains the implemented model training by aligning different data sources.
	- `Screening.ipynb` contains information about screening of possible candidates based on the Modeling notebook.
	
	
## Usage
- **Data Acqusitions**: Several formats of data collection methods are devised from API calls, direct downloads, webcrawler through Scholarly articles.
- **Data Processing**: Parses CIFs, classifies perovskites (hybrid/pseudohalide), imputations and feature engineering portions.
- **Model Training**: Fits XGBoost/NN ensemble; generates predictions with uncertainty.
- **Outputs**: Parity plots, feature importance (SHAP), top candidates, etc., in `outputs/`.

*More indepth discussion about the usage in the research draft.*

## Key Considerations
- **Bandgap Physics**: 
	Model incorporates B-site effects on conduction band and X-site anion size/electronegativity.
- **Pseudohalides**: 
	Larger anions increase volume, reducing Madelung energy and bandgap; synthetic data uses radii from Shannon tables and literatures for accuracy .
- **Limitations**: 
	Assumes ABX₃ stoichiometry. Need to assess with real-world experimental setup. Further considerations needed for any screened candidates from the modeling to experimentations.
- **Validation**: 
	Use DFT (e.g., via pymatgen) for predicted candidates; uncertainty guides experimental prioritization. Requires structural modeling using the compound structures to create inferences.

Contact: kngbq@missouri.edu for collaborations on perovskite ML.
