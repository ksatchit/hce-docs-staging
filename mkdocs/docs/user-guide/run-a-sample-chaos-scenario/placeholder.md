# Run A Sample Chaos Scenario 

## Objective

- Run your first chaos scenario (experiment) on a sample microservices application using HCE  
- Familiarise yourself with the scenario (experiment) execution flow in the HCE platform

## Before You Begin

- This exercise makes use of [`podtato-head`](https://github.com/podtato-head/podtato-head), a sample microservice application as the chaos target.
- We will utilize a pre-defined demo chaos scenario from the Enterprise ChaosHub (an artifact source containing chaos experiment and scenario definitions) that will install the application, subject it to chaos for a fixed time period and then cleanup the application. 
- The experiment will leverage the auto-installed Chaos Delegate `Self-Agent` to carry out the chaos
 
  **Note**: The application install and cleanup steps are generally not part of chaos scenario definitions, and have been included in this case purely for illustrative purposes. 

To learn more about [ChaosHub](https://ksatchit.github.io/hce-docs-staging/technical-reference/content/placeholder/#summary) and [Chaos Delegate](https://ksatchit.github.io/hce-docs-staging/technical-reference/content/placeholder/#summary), refer to the respective sections in technical reference documentation

## UseCase 

- The podtato-head application consists of several microservices, each employed as a stateless Kubernetes Deployment, typically with multiple replicas each. 
- The chaos experiment involves **random kill of a replica pod** belonging to the main microservice `podtato-main` (which is responsible for rendering the  image of podtato-head on the web browser)
- The experiment also performs the auto-validation of the hypothesis that **the podtato-head image is always rendered successfully and the web page is always accessible**, notwithstanding the failure of a replica pod.

## Steps

#### Step-1: Schedule a New Scenario

- In Harness Chaos Engineering ChaosCenter, click **Chaos Scenarios**, and then click **Schedule a Chaos Scenario**. The Scenario wizard appears.

#### Step-2: Choose a Chaos Delegate

- Select the Chaos Delegate named **Self-Agent**. The Chaos Delegate connects the HCE control plane with the Kubernetes clusters which hosts your target applications/chaos subjects. The Self-Agent is a Chaos Delegate running on the same cluster as the HCE control plane. 

**Note**: If the Self-Agent does not appear, the related Kubernetes workload (a deployment named `subscriber`) might not be in an active state. Wait a few minutes for it to reach steady state. If, after a while, it is still inactive, ensure that ingress to the litmusportal-server-service NodePort is not restricted and a firewall exception is present for that NodePort, if applicable. 

- Once you've selected a Delegate, click **Next**.

#### Step-3: Choose a Scenario

- Click **Create a new scenario from one of the pre-defined chaos scenario templates**. The ChaosHubs appear.

  Select **Litmus ChaosHub**.

  In search, enter **podtato-head**

  ![image](https://user-images.githubusercontent.com/21166217/192180454-d5736e66-7e4a-49e0-9589-d1281abccf78.png)

- Select **podtato-head** and then click **Next**.

- In **Chaos Scenario Settings**, you can enter the Scenario name and the target namespace in the cluster. This predefined Scenario template runs in the **litmus** namespace.

- Click **Next**.

#### Step-4: Tune the Scenario

- In this case, you can see the steps of the Scenario are already defined. Double-click on the diagram to zoom in.

  Here are the steps:
  
  - install-application: Installs the podtato-head application (it is removed later by delete-application).
  - install-chaos-experiments: Installs the Chaos Experiments (fault templates) in the Scenario. Here, it comprises pod-delete.
  - pod-delete: This experiment deletes target pods at given intervals for a specific period. The recovery process of the deleted pods is taken care of by Kubernetes.
  - revert-chaos: Removes the resources created (at runtime) to carry out the scenario execution. 
  - delete-application: Deletes the podtato-head application.

- Continue to walk through the wizard 

  ![image](https://user-images.githubusercontent.com/21166217/192181248-f9f5fa2f-3e3c-4d30-b9c3-06de8c47198c.png)

  **Note**: In Target Application, in appns, you can see the target namespace for the experiment. In this Scenario, the target namespace is {{workflow.parameters.adminModeNamespace}}. The application isn't installed yet, so this runtime variable will resolve to the actual namespace where the application is installed.
  
  In **Define the steady state for this application**, you can see a probe defined for this experiment. This probe tests the steady-state hypothesis that the podtato-head website continues to be availabile during the execution of the experiment. 
  
  Click **Show Details** for a quick look at the Probe
  
  ![image](https://user-images.githubusercontent.com/21166217/192181621-29eb8c17-9e8a-41b9-b104-de01e60ff4e0.png)

  In the details, you can see valuable information:

  - URL: The application URL that you can use to view the application when the experiment is running
  - Method: This is the criteria Harness will evaluate continuously during the experiment (see *Continuous* under Mode). In this case, HTTP GET requests are made to the URL and Harness Chaos Engineering verifies if it received an HTTP 200 response.

  Refer the [technical reference](https://ksatchit.github.io/hce-docs-staging/technical-reference/content/placeholder/) documentation to learn more about Probes. 
  
#### Step-5 Adjust the Resiliency Score

- In this step, set a "weightage" for the pod-delete experiment within the scenario. This contributes towards the "Resilience Score" obtained for the scenario at the end of its execution. 
 
  Resiliency Score is the measure of how resilient your Scenario is considering all the chaos experiments and their individual result points. The successful outcome of each test in a Scenario carries a certain weight, configured at this stage. This is typically useful when the scenario consists of multiple experiments. 
  
  You can adjust the weights of the experiments in the Scenario by dragging the slider.
  
  ![image](https://user-images.githubusercontent.com/21166217/192182365-badc9b51-eeb8-4cdb-8e23-2e4f286b29f0.png)

  For this exercise, just keep the default Resiliency Score.
  
- Click **Next**.

#### Step-6: Choose a Schedule

- You can schedule the Scenario to run once, or run it on a recurring schedule.

  Click **Schedule now**, and click **Next**.

#### Step-7: Run the Scenario 

- Chaos Center shows a summary of the Scenario.

  ![image](https://user-images.githubusercontent.com/21166217/192182503-3a05e6ab-d969-410a-b95e-13d387ff80bc.png)
  
  At this, stage you could click **edit** to modify any of the settings. For now, we'll leave the defaults. 

- Click **Finish**

- Click the **Go to Chaos Scenario** link to see the Scenario in action, or click **Scenarios** in the navigation to see it running.

  ![image](https://user-images.githubusercontent.com/21166217/192182913-46c1d32b-650a-4e37-b7a7-51d0bef3f6f0.png)
  
- Click the name of the Scenario to visualize its progress.
  
  ![image](https://user-images.githubusercontent.com/21166217/192183040-f75a14a9-06a6-4216-bb25-80b4f85c2ee1.png)  
  
  **Note**: Logs can be accessed for the experiment by clicking on the pod-delete step node. The logs will be available
  for a temporary period, i.e., until the **revert-chaos** step is executed. 

#### Step-8: View the Results

- Once the scenario is completed, click the **pod-delete** step in the graph view & view the **Chaos Results** 

  ![image](https://user-images.githubusercontent.com/21166217/192183736-3a28c94c-6919-47b6-84a0-459ca3c78e38.png)

- You can see the success of the chaos experiment.

  ```
  Experiment Verdict: Pass
  Phase: Completed
  Fail Step: N/A
  ```
  
  Click **Probe Result** to view the success/failure of the steady-state hypothesis constraint (podtato-head website availability through pod deletion period):
  
  ```
  Probe Success Percentage: 100%
  Probe Status: 
  Name: check-podtato-main-access-url
  Type: httpProbe
  Continuous: "Passed 👍 "
  ```
  
## Summary

- You have successfully executed your first chaos scenarion run using HCE
- Since the podtato-main service was deployed with multiple replicas, our hypothesis that the webpage continues to stay accessible despite killing one of the replicas was validated to be accurate. 

# Next Steps

- Connect remote Chaos Delegates to the Chaos Center to discover workloads and carry out chaos scenarios on other Kubernetes clusters 
