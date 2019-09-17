# CarBreakDown
## Assumpitons 
1. All **100 vehicles** are assumed to be different and has been treated in a individual manner.

## Different approaches which can be used for predicting breakdown
Out of 21 sensors if we find rolling mean/mean or plot the sensor data, only s2,s3,s4,s7,s8,s9,s11,s12,s13,s14,s15,s17,s20,s21
are observed to change with the day while others are either constant or have mininmal deviation.

  **Method 1 :** Out of filtered sensors there is either a dip in values or a lift in values, a rolling mean would of 2-4 days would
  serve the purpose, for each sensors ~30 days proir the trends starts changing, we can mark the thresholdd values over which the
  flag would be raised for each sensor and if majority of the sensors shows the threshold breach we can have a breakdown indicator.
  
  **Method 2 :** There could be a more robust way for this, what we have tried is fitting an ensemble based boosting algorithm 
  which can give slightly extra confidence and would also tell the probaility of the failure prediction. I have treated all the
  vehicles differently. A **target variable is defined** in a manner where for each vehicle, corresponding to the day, either 0 or 1 
  has been marked. Say for example a vehicle lasts 150 days, till 120 days the target variable is 0 and post that its 1, this 
  number couls have been chamnged (but the requirement was to predict failure around 30 days). Instead of predicting 0 and 1 
  we have predicted the probabilty of day's sensor data pattern bieng close to 0 or 1, which would give a better picture of whatever
  we are predicting.
  
  If we assume ground data to be true (which is not satisfactorily true, as even after plotting the raw sensors data over days, in
  many cases no trends are observed which points towards the breakdown as observed in train data), about 25 vehicles out of 100 had
  RUL around 30, and out of 25 vehicles the model could predict the failure for 20 vehicles with over 0.50 probability, and also 22
  vehicles out of 25 at some point showed trend towards breakdown but didnt fail eventually. 
  
  So precision was observed to be ~80%, assuming the ground data is true. False positives was an focused more, as that would impact
  business a lot, telling a vehicle wuld fail and blockig it for maintainace would decrease the revenue from thta car.
  
  **dfAccuTrain** : Contains accuracy observed obe training 100 models over 100 cars.   
  **dfAccuTestl** : Contains accuracy over test data with probability threshold of 0.50 and higher, after looking 
                    at the end days of data
  **dfInd**       : contains flag if at any point of time over all the days of test data, if any patterns similar to the breakdown
                    vehicle data was observed (36 vehicle showed flag,  28 to be dominant flags and 8 as minor flags)
                    
  **Method 3** : We can calculate **feature importance scores** based on the sensors and figure out the importance order of the
  sensors and assosiate the proportional scores to the sensors, to predict the breakdown of a vehicle.
  
  **Method 3** : Simple classification model (k-means should work for this king of data) can also be used, **good thing is manual 
  marking will not be required** for this, we can assume 3 clusters, 1 for a healthy vehicle pattern, other for degrading vehicle
  and the 3rd one fot the last stage of breaking down vehicle.
  
  **Method 4**: If we had large amount of data nueral metwork classifiers could also be used, which could have yielded a higher
  acuracy.
  
  **Method 5 :** A more complex but yet quite reliable would be using HMM's, it would be usefull for this king of data as its a time
  series data, and hidden staes could be marked as 3 which is good health, average health and bad health, just that sample points to
  train are less than ideal to start with.
