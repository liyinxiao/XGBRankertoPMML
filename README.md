# XGBRanker2PMML


The existing sklearn2pmml library (https://github.com/jpmml/sklearn2pmml) does not support XGBRanker. The goal is to slightly modify sklearn2pmml library (with minimum efforts), to get around it and support converting XGBRanker model pipeline to pmml. It supports objective='rank:pairwise' and objective='rank:ndcg'.


How to convert a XGBRanker to pmml: 

* pip install sklearn2pmml==0.46.0
* Replace jpmml-xgboost-1.3-SNAPSHOT.jar in site-packages/sklearn2pmml/resources
* model training

```
gbm = XGBRegressor(learning_rate=0.1)
gbm_dummy = XGBRanker(learning_rate=0.1, objective='rank:ndcg')
gbm_dummy.fit(x, y, group)
gbm._Booster = gbm_dummy._Booster
pipeline = Pipeline([
 ("predictor", gbm), 
])
pmmlpipeline = make_pmml_pipeline(pipeline, target_fields=['score']) 
sklearn2pmml(pmmlpipeline, "Model.xml")
```
