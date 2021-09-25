
Overall goal is the creation of a template micromanager that others can easily modify

### Permissions

 - Use [muid](https://github.com/microprediction/muid) as is. 

### Eyeballs

 - Implement [microboards](https://github.com/microprediction/microboards) utilities for exposing tables & leaderboards nicely using some of the front end code at [microprediction-site](https://github.com/EricZLou/microprediction-site)

### Calculations & scoring conventions 

 - Separately implement rolling [lottery](https://github.com/microprediction/lottery) calculations to avoid crude elsewhere. 

### Endpoint deployment

 - Most likely integrate with [functions-framework-python](https://github.com/GoogleCloudPlatform/functions-framework-python) and create the template in [microfunction](https://github.com/microprediction/microfunction) with utilities in [microfunctions](https://github.com/microprediction/microfunctions) 

### Responder registry

A mix of software responders and access to 
 - Create [microregistry](https://github.com/microprediction/microregistry) similar to [offline](https://github.com/microprediction/offline) that periodically polls a database. 
 - Also makes available the subset registered as ONNX models with a certain signature. 

### Responder types

Initially there are only two types, though likely more will be beneficial

### iskater (stateless, point estimate)

Maps list of float -> float and does not learn 
- Typically used for point estimates of time series, but could be any operation R^n->R
- Typically uses onnx runtime
- Deployed via google cloud functions
- Pre-trained and does not learn
- Doesn't look at meta-data 

### recommender (stateful, weighted samples)

When supplied a finite list of MUIDs, and meta data dict, will enter a lottery with enumerated allowed_values
- Expected to learn as it goes 
- Looks at meta: allowed_values to decide if it is recommending or just choosing 



 
 
### Higher-level abstractions 

 - Create [microratings](https://github.com/microprediction/microratings) utilities to show how [Elo-ratings](https://microprediction.github.io/timeseries-elo-ratings/html_leaderboards/univariate-k_003.html) are a special case




