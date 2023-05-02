# zindi-earlylearning-py


## Introduction

This repository contains the replication files for the Zindi Competition DataDrive 2030 Early Learning Predictors. For this task, I tune and fit a LightGBM [6] model. The local explainability of model predictions is given through Shap values [12]. Global model sensitivity to changes in features is provided via feature shuffling. Note: the Shap and sensitivity metrics provided should *not* be interpreted causally as this requires a causal framework. This is a purely predictive modelling approach (and competition) and the models should therefore be understood as such. 

Alternatively, an exploratory modelling approach such as in [2, 5] with variable selection controls [8, 9] would be interesting to explore. Also, counterfactual explainability as in [11].

For the competition see here: [https://zindi.africa/competitions/datadrive2030-early-learning-predictors-challenge](https://zindi.africa/competitions/datadrive2030-early-learning-predictors-challenge).


## Repository & Workflow 

### /data/

All data are included in the repository. The original data from Zindi is in `/competition/` and `/clean/` contains the preprocessed version (see below). 


### /notebooks/

The notebooks, in their respective order: 

-- **data-prep.ipynb**: Some preliminary data cleaning and pre-preprocessing. I do not do extensive feature engineering, rather I fix some numerical/categorical mixups, multiple answers columns, drop features with too many missings, feature engineer the date columns, and for a few categorical variables add an ordinal encoding.  

-- **tuning-bo.ipynb**: I tune the LightGBM model w.r.t. percentage of considered features per iteration and the two categorical hyper-parameters. The learning rate is fixed low and the number of iterations selected by early stopping with 5 patience rounds. Ideally, generalisation performance would be assessed by resampling techniques, here not considered due to the computational demands (and not expected improvements). This is run for 45min with Bayesian Optimisation using the Tree Parzen Estimator in Optuna. 

-- **analysis.ipynb**: Given the tuned hyper-params, I rerun the model, select the number of iterations using 10-fold cv and refit the model on the complete training datasets. Shap values are computed and a submission file is generated.

-- **sensitivity.ipynb**: Added sensitivity analysis with feature shuffling to assess model sensitivity to individual features. This is not in the scope of the sensitivity analysis requested in the competition. First, the mean absolute errors in this competition are very high, indicating a very low signal-to-noise ratio. Second, one might be able to compute changes in the prediction conditional on changes in the input, for example using partial independence plots. Though, this does not mean this is useful, much less actionable information. As noted above, this is a predictive framework, not a causal one. 

Note, since the data and tuned parameters are provided, it is not necessary to rerun all notebooks. 


### /notebooks-test/

The folder contains some test runs on feature selection and advanced hyperparameter tuning algorithms [1, 3], but are not relevant to the submitted results.


### /submission/

Contains the submitted results file. Note, if the scripts are rerun, the predictions will be saved in a csv file with the corresponding date and time stamp.


## Some learnings (informally)

- hyper-param tuning in noise environments (even with cross-validation) is not useful
- categorical encodings can make & break analyses and it is far from consistent across packages (in particular python)
- ideally one does feature selection beforehand, but this is difficult in this case given many categorical covariates and a high share of missings. feature sensitivity by shuffling might have been an option. 

Other concerns:
- tree-based methods that allow for missing values might be biased towards covariates with less missings (as missings are not counted towards the gain when splitting), so if some surveys are stitched together we might only learn from a few surveys effectively discarding the others. this might introduce bias in the considered population.


## Reproducibility and computational requirements

All notebooks were run on a 2019 i5 laptop. Tuning is the most computationally heavy part, which is timed for 45 min. In particular, a different machine may yield a higher or lower number of trials and thus a (slightly) different tuning result. Therefore the tuning and analysis spart is split. A `requirements.txt` is included to reproduce the environment that was used to run the included notebooks. All analyses were done in Python `v3.10.11`. 

Tuning and data preparation are included for reference and are not necessary to run again to replicate the analysis. The `analysis.ipynb` fits the model and computes the predictions for the submission and should run in a few minutes, the sensitivity notebook in even less.


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

[11] R. K. Mothilal, A. Sharma, and C. Tan, ‘Explaining machine learning classifiers through diverse counterfactual explanations’, in Proceedings of the 2020 Conference on Fairness, Accountability, and Transparency, Barcelona Spain: ACM, Jan. 2020, pp. 607–617. doi: 10.1145/3351095.3372850.

[12] S. M. Lundberg and S.-I. Lee, ‘A unified approach to interpreting model predictions’, in Proceedings of the 31st international conference on neural information processing systems, in NIPS’17. Red Hook, NY, USA, 2017, pp. 4768–4777.
