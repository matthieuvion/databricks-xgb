## Databricks ML quickstart notebook, scaled up
 
Adaptation of Databricks notebook "ML training".
- Replace (almost) everything Pandas/Sklearn related by their Spark/Xgboost counterparts
- Ensure compatibility with Community Edition (15gb, 2 workers)


**Inside:**
- Part 1: Training classification model using the "new", distributed implementation of xgboost : `spark.xgboost`. We also use gridsearch & Spark `Pipeline()`
- Part 2: Better hyperparameters tuning with "classic" xgboost, `Hyperopt`+SparkTrials, as well as tracking performance using `MLflow`.

**Changes applied:**  

*Compatibility with community edition:*
- In Community edition the /dbfs/ mount point isn't available. We "fix" that by copying the datasets to a local folder 

*Scale up:*
- **Tl,dr:** replace everything pandas/sklearn-related with their counterparts
- Does not really need it, as we have a small dataset ;)
- Load file and transformations with Spark (-> spark.dataframe) instead of Pandas
- Preprocessing using Spark functions (instead of Pandas)
- Remove everything related to Scitkit-learn, replace with pyspark.ml or xgboost lib
- e.g xgboost or xgboost.spark instead of sklearn GradientBoostingClassifier
- Streamline our modeling workflow, using Spark Pipeline()

*What we keep from the original notebook:*
- General flow / goals / examples
- Particularly : MLFlow tracking & Distributed params optimization with Hyperopt + Sparktrials

*Remarks on xgboost + MLFlow tracking as of July 2023:*
- This might help you if lost with all xgboost implementations + compatibility:   
- Your choices for XGB: classic `xgboost`, `xgboost.spark` (new, distributed) or sklearn `GradientBoosting` or `sparkdl.xgb`
- sparkdl soon deprecated, xgboost.spark not (yet?) compatible with MLFlow autolog, and overall it seems a real mess to perform search grid & cv in conjunction with MLFlow
- In **part 1.** original notebook uses sklearn + MLFLow on successive runs of models with different params. **We replace it with xgboost.spark**, without MLFlow, but with a `pipeline()` and hyperparams optimization.
- If you still wanted to monitor with MLflow, you could use xgboost classic lib, with mlflow.xgboost as shown [here](https://docs.databricks.com/_extras/notebooks/source/mlflow/mlflow-end-to-end-example.html)
- In **part 2.** original notebook uses sklearn + grid search with Hyperopt.SparkTrials, we **replace sklearn with xgboost** lib and keep optimization w/ `hyperopt + SparkTrials`.  