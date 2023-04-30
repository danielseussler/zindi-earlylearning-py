# zindi-earlylearning-py


## Introduction

This repository contains the replication files for the Zindi Competition DataDrive 2030 Early Learning Predictors. For this task, I tune and fit a LightGBM model. Explainability of model predictions is given through Shap values. Note: the Shap values provided should *not* be interpreted causally as this requires a causal framework. This is a purely predictive modelling approach (and competition) and the models should therefore be understood as such. 

Alternatively, an exploratory modelling approach such as in [2, 5] with variable selection controls [8, 9] would be interesting to explore further. 

For the competition see here: [https://zindi.africa/competitions/datadrive2030-early-learning-predictors-challenge](https://zindi.africa/competitions/datadrive2030-early-learning-predictors-challenge).


## Repository and Reproducibility

The main notebook of interest is `/notebooks/analysis.ipynb` where the main model is fitted and the Shap values are computed. The hyperparameters used there were found using Bayesian Optimization [4, 10] in `/notebooks/tuning-bo.ipynb`. Some simple data cleaning and recoding were done in `/notebooks/data-prep.ipynb`. Tuning and data preparation are included for reference and are not necessary to run again to replicate the analysis. All notebooks were run on a 2019 i5 laptop. Tuning is the most computationally heavy part, which is timed for 45 min. `/submission/` contains the submitted files. The repository includes all necessary data. A `requirements.txt` is included to reproduce the environment that was used to run the included notebooks. All analyses were done in Python `v3.10.11`. 


## References 

[1] S. Falkner, A. Klein, and F. Hutter, ‘BOHB: Robust and Efficient Hyperparameter Optimization at Scale’. arXiv, Jul. 04, 2018. Accessed: Apr. 22, 2023. [Online]. Available: http://arxiv.org/abs/1807.01774

[2] P. Bühlmann and B. Yu, ‘Boosting With the L 2 Loss: Regression and Classification’, Journal of the American Statistical Association, vol. 98, no. 462, pp. 324–339, Jun. 2003, doi: 10.1198/016214503000125.

[3] N. Awad, N. Mallik, and F. Hutter, ‘DEHB: Evolutionary Hyperband for Scalable, Robust and Efficient Hyperparameter Optimization’. arXiv, Oct. 21, 2021. Accessed: Apr. 22, 2023. [Online]. Available: http://arxiv.org/abs/2105.09821

[4] B. Bischl et al., ‘Hyperparameter optimization: Foundations, algorithms, best practices, and open challenges’, WIREs Data Min & Knowl, vol. 13, no. 2, Mar. 2023, doi: 10.1002/widm.1484.

[5] N. Fenske, T. Kneib, and T. Hothorn, ‘Identifying Risk Factors for Severe Childhood Malnutrition by Boosting Additive Quantile Regression’, Journal of the American Statistical Association, vol. 106, no. 494, pp. 494–510, Jun. 2011, doi: 10.1198/jasa.2011.ap09272.

[6] G. Ke et al., ‘LightGBM: A Highly Efficient Gradient Boosting Decision Tree’, in Advances in Neural Information Processing Systems, Curran Associates, Inc., 2017. [Online]. Available: https://proceedings.neurips.cc/paper_files/paper/2017/file/6449f44a102fde848669bdd9eb6b76fa-Paper.pdf

[7] T. Akiba, S. Sano, T. Yanase, T. Ohta, and M. Koyama, ‘Optuna: A Next-generation Hyperparameter Optimization Framework’, in Proceedings of the 25th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining, Anchorage AK USA: ACM, Jul. 2019, pp. 2623–2631. doi: 10.1145/3292500.3330701.

[8] N. Meinshausen and P. Bühlmann, ‘Stability selection: Stability Selection’, Journal of the Royal Statistical Society: Series B (Statistical Methodology), vol. 72, no. 4, pp. 417–473, 2010, doi: 10.1111/j.1467-9868.2010.00740.x.

[9] R. D. Shah and R. J. Samworth, ‘Variable selection with error control: another look at stability selection: Another Look at Stability Selection’, Journal of the Royal Statistical Society: Series B (Statistical Methodology), vol. 75, no. 1, pp. 55–80, Jan. 2013, doi: 10.1111/j.1467-9868.2011.01034.x.

[10] J. Bergstra, R. Bardenet, Y. Bengio, and B. Kégl, ‘Algorithms for hyper-parameter optimization’, in Advances in neural information processing systems, 2011. [Online]. Available: https://proceedings.neurips.cc/paper_files/paper/2011/file/86e8f7ab32cfd12577bc2619bc635690-Paper.pdf
