# Install the Harness Chaos Engineering Platform

## Objectives

- Install Harness Chaos Engineering in a Kubernetes cluster.

## Before You Begin

- Kubernetes cluster:
  - A general purpose compute engine like GCP e2-standard-2 is sufficient.
  - Kubernetes >= 1.21.
  - A default storage class with 20GB usable disk space for the Persistent Volume

- Helm 3 installed in the cluster. See Installing Helm from Helm.

## Install Options 

### helm

- Add the Harness Helm repository:

  ```
  helm repo add harness https://hce.chaosnative.com
  ```
  
- View the repo: 

  ```
  helm repo list
  ``` 
  
  Output:

  ```
  NAME     URL
  harness  https://hce.chaosnative.com 
  ```

- Update the Harness Helm repo:

  ```
  helm repo update
  ```
  
  Output:
  
  ```
  Hang tight while we grab the latest from your chart repositories...
  ...Successfully got an update from the "harness" chart repository
  Update Complete. ⎈Happy Helming!⎈
  ```
  
- Install Harness Chaos Engineering ChaosCenter:

  ```
  helm install -n litmus hce harness/hce --create-namespace
  ```

  Output: 
  
  ```
  NAME: hce
  LAST DEPLOYED: Wed Aug  3 16:27:37 2022
  NAMESPACE: litmus
  STATUS: deployed
  REVISION: 1
  TEST SUITE: None
  NOTES:
  Thank you for installing hce 😀

  
  Your release is named hce and it's installed to namespace: litmus.

  Visit https://harness.io to find more info.
  ```

### kubectl 

- Install Harness Chaos Engineering with `cluster-admin` permissions in the litmus namespace by default.
 
  ```
  kubectl apply -f https://hce.chaosnative.com/manifests/latest/hce-cluster-scope.yaml
  ```

## Verify Installation

- Verify that all pods are brought up successfully in the `litmus` namespace

  Now, the HCE control plane is successfully brought up in your Kubernetes cluster. To learn more about the various control plane components, 
  refer the [technical reference](https://ksatchit.github.io/hce-docs-staging/technical-reference/content/placeholder/) section. 

## Access ChaosCenter

- By default, Harness Chaos Engineering is accessed using a LoadBalancer. View the LoadBalancer IP (this can take a few minutes):

  ```
  kubectl get svc -n litmus
  ```
  
  Example output using a helm install: 
  
  ```
  NAME                      TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                         AGE
  hce-auth-server-service   NodePort       10.24.5.26     <none>          9003:32156/TCP,3030:31024/TCP   2m28s
  hce-frontend-service      LoadBalancer   10.24.15.215   34.173.193.67   9091:32631/TCP                  2m28s
  hce-headless-mongo        ClusterIP      None           <none>          27017/TCP                       2m28s
  hce-mongo                 ClusterIP      10.24.14.6     <none>          27017/TCP                       2m28s
  hce-server-service        LoadBalancer   10.24.11.20    34.67.119.129   9002:31108/TCP,8000:30292/TCP   2m28s
  license-service           NodePort       10.24.13.205   <none>          80:31560/TCP                    2m28s
  ```
  
  The EXTERNAL-IP might say pending for a few minutes while the load balancer is set up.
  
- Simply use your EXTERNAL-IP and PORT for the hce-frontend-service in this manner <EXTERNAL-IP>:<PORT> to access Harness Chaos Engineering: http://<EXTERNAL-IP>:<PORT>. For example:
  
  ```
  http://34.71.48.119:9091/
  ```
  This is an example IP and port. Yours will be different.
  
  ChaosCenter is displayed in your browser:
  
  ![image](https://user-images.githubusercontent.com/21166217/192174707-8d4c1338-d07a-4b8a-af66-68b37bc40206.png)

- Log in with the default credentials:
  
  - Username: admin
  - Password: litmus
  
  Once you log in, you'll be asked to set a new password.  

## License Activation

- Next, upload your license file and click Activate License.

  You can request a license from the [Harness Chaos Engineering sign up page](https://harness.io/trial/che-on-prem).
  
- Drag and drop your license into Upload License File and click Activate License.
  
  ![image](https://user-images.githubusercontent.com/21166217/192174887-10bf9d53-6bbc-4732-9819-8feaea5e16d5.png)

  You're ready to go.



