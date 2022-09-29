# Define Hypothesis Using Probes

## Objective

## Before You Begin

## Steps

#### Step-1: Add a Probe To The Selected Experiment 

**Note**: In this exercise we shall configure a HTTP Probe to query the availability of the application under test. 
For this purpose, we will leverage the FQN of the Kubernetes service associated with the application workload. 

- Click **Add a New Probe** 

  <img width="1427" alt="Screenshot 2022-09-29 at 11 08 35 AM" src="https://user-images.githubusercontent.com/21166217/192947698-2bf665f5-83f0-4085-8e86-bd6c11349748.png">


#### Step-2: Configure the Probe

- Configure the probe with the following inputs: 

  - Provide an appropriate name for the probe against the **Probe Name** 
  - Select _http_ amongst the list of **Probe Types**
  - Select _Continuous_ amongst the list of **Probe Modes**
  - Define run behaviour, including polling intervals and retry settings for failures, in the **Probe Properties**. 
    **Note**: The example shown below comprises typical values used when querying local services. 
  - Provide the (service) endpoint in the URL with a desired HTTP response timeout value (in ms)
  - Define the HTTP request type and the constraint/criteria to determine probe success
  - Click **Add Probe** once the configuration is completed

  <img width="531" alt="Screenshot 2022-09-29 at 11 07 31 AM" src="https://user-images.githubusercontent.com/21166217/192947540-8b84f667-d7da-4f80-a73c-bb197a6bb8b2.png">
  
#### Step-3: Execute the Scenario 

- Complete the rest of the configuration steps for the experiment and launch the scenario

#### Step-4: View the Probe Results 

- <img width="1434" alt="Screenshot 2022-09-29 at 11 40 29 AM" src="https://user-images.githubusercontent.com/21166217/192952691-2e550efa-225a-430b-bada-2d7cbc462aed.png">

#### Step-5: Verify Experiment Verdict Based on Probe Results

<img width="952" alt="Screenshot 2022-09-29 at 11 42 14 AM" src="https://user-images.githubusercontent.com/21166217/192952942-d47ea142-e9c4-42b2-a5d7-043553525e42.png">




 
 
