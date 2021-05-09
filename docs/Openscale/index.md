# Monitoring with IBM Opescale

### Setting Up the Enviroment
1. Creating Database

2. Creating Machine Learning Provider

    On the Machine Learing Provider Tab, click on the ```Add machine learning povider buttonr```.
    <div style="text-align:center"><img src="../assets/Openscale/Capture2.PNG" alt="drawing" /></div>

    2.1 Add a name and description.

    <div style="text-align:center"><img src="../assets/Openscale/Capture3.PNG" alt="drawing" /></div>

    2.2 Add connetions and Select Deployment Space
    Under ```Service Provider```, select ```Watson Machine Learning (V2)``` from the dropdown. Next select the deployment space your model is located in.
    <div style="text-align:center"><img src="../assets/Openscale/Capture4.PNG" alt="drawing" /></div>
  
    2.3

3. Adding to Dashboard
    3.1 On the ```Insights Dashboard```, click on the ```Add to dashboard``` button.
    <div style="text-align:center"><img src="../assets/Openscale/Capture5.PNG" alt="drawing" /></div>
    3.2 Next select the proveider you just created, then select your model deployment and click on ```Configure``` and then ```Configure Monitors```.

     <div style="text-align:center"><img src="../assets/Openscale/Capture6.PNG" alt="drawing" /></div>

    3.3 Select the data and Algorithim types, in our example it is a Binary Classification.

    3.4 The next step is selecting the training data that can be stored on a Db2 database or in IBM's Cloud Object Storage.

     <div style="text-align:center"><img src="../assets/Openscale/Capture7.PNG" alt="drawing" width=50%/></div>

    3.5 Now select the Label column (column you want to predict).
    
    3.6 Next we select all the features we want to include as well as indicate which ones are categorical.
    <div style="text-align:center"><img src="../assets/Openscale/Capture8.PNG" alt="drawing" width=50%/></div>

    3.7 Here we can select Automatic Logging.

    3.8 Finally we can select ```prediction``` and ```probability``` for the model output.details

4. Configuring Monitors
    We can create monitors for ```Fairness```,  ```Quality```. ```Drift``` and ```Explainability```.

    4.1 __Fairness:__ The monitor checks your deployments for biases. It tracks when the model shows a tendency to provide a favorable (preferable) outcome more often for one group over another. 

    We have to specify which values represent favorable outcomes and then select the features to monitor for bias, in our case we chose to monitor extreme temperatures in the ```MinTemp``` and ```MaxTemp``` columns.
     <div style="text-align:center"><img src="../assets/Openscale/Capture9.PNG" alt="drawing" width=50%/></div>

    4.2 __Quality:__ This monitor evaluates how well the model predicts accurate outcomes that match labeled data. It identifies when model quality declines, so we can retrain your model if needed.

    We can set the Quality Threshold value, which Area under ROC, at 0.8.

    4.3 __Drift:__ The drift evaluation measures drop in accuracy by estimating the drop in accuracy from a base accuracy score determined by the training data and also drops in data consistency, by estimating the drop in data consistency by comparing recent model transactions to the training data.

    We can set the Drift threshold as 20%.

    4.4 __Explainability:__ This allows us to reveal which features contributed to the model’s predicted outcome for a transaction and suggests what changes would result in a different outcome.
    
    We can choose all features an controllabe.
    
### Logging 
In the ```Transactions``` page, we can see informations about transactions, including a Timestamp, Prediction and Confidence.

 <div style="text-align:center"><img src="../assets/Openscale/Capture10.PNG" alt="drawing" width=100%/></div>

### Evaluating 

### Explaining Predictions
Again in the ```Transactions``` page, we can click on the ```Explain``` button, in the following page we can observe each features' relative weight indicating how strongly they influenced the model’s predicted outcome.
 <div style="text-align:center"><img src="../assets/Openscale/Capture11.PNG" alt="drawing" width=100%/></div>

In the ```Inspect``` tab, there is a table displaying the values aech feature would have to have to alter the prediction result, here we can also change the values by hand to see what the outcome would be.

 <div style="text-align:center"><img src="../assets/Openscale/Capture12.PNG" alt="drawing" width=100%/></div>
