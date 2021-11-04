
Overall goal is the creation of a template micromanager that others can easily modify

## Permissions

 - Use [muid](https://github.com/microprediction/muid) as is. 

## Eyeballs

 - Implement [microboards](https://github.com/microprediction/microboards) utilities for exposing tables & leaderboards nicely using some of the front end code at [microprediction-site](https://github.com/EricZLou/microprediction-site)

## Calculations & scoring conventions 

 - Separately implement rolling [lottery](https://github.com/microprediction/lottery) calculations to avoid repeating elsewhere. 

### Endpoint deployment

 - Most likely integrate with [functions-framework-python](https://github.com/GoogleCloudPlatform/functions-framework-python) and create the template in [microfunction](https://github.com/microprediction/microfunction) with utilities in [microfunctions](https://github.com/microprediction/microfunctions) 

### Responder registry

A mix of software responders and access to 
 - Create [microregistry](https://github.com/microprediction/microregistry) similar to [offline](https://github.com/microprediction/offline) that periodically polls a database. 
 - Also makes available the subset registered as ONNX models with a certain signature. 
 - BLOCKER: SKLEARNED not really working that well

## Responder types

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
- Looks at meta: allowed_values to decide if it is recommending or just choosing, when it is refered to as a "chooser" 

### Benchmark iskater 

We provide a benchmark iskater that makes self-referential use of the ecosystem. It is parameterized by a depth parameter
which controls the depth of the rabbit-hole, and also a timescale parameter that warns us from leaving the microprediction domain. 

If depth or timescale do not permit, or the set of suppliers is empty: 
0. Uses a forever function to return answer with no fan-out

Otherwise...

1. Receives request for point estimate and fans out to suppliers
2. Stacks the supplier estimates and returns and answer

Periodically... 

4. Requests recommendation for suppliers, where the supervision data is success in future epochs (-depth, timescale+)
5. Requests choice once suppliers are shortlisted, where the supervision data is success in future epochs (-depth, timescale+) 
6. Peridically solves the cost-aware race-manager problem, by using the results of (3) and the horse race algorithm to determine relative ability. 
7. Collates market and other data on supplier performance, and requests point estimate of their future accuracy (-depth, timescale+)

### Benchmark recommender

Very similar to the benchmark iskater except the top-level task is recommendation. 
 
 
## Higher-level abstractions (applications) 

 - Create [microratings](https://github.com/microprediction/microratings) utilities to show how [Elo-ratings](https://microprediction.github.io/timeseries-elo-ratings/html_leaderboards/univariate-k_003.html) are a special case




