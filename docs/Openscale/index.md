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


### Creating Monitors

### Logging 

### Evaluating 

### Explaining Predictions