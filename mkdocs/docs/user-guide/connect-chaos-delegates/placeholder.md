# Connect Remote Chaos Delegates to Chaos Center

## Objective 

- Connect cluster-scoped OR namespace-scoped Chaos Delegates to enable chaos experimentation on remote Kubernetes clusters. 

## Before You Begin 

- Ensure that you have [installed the Harness Chaos Engineering Control Plane](https://ksatchit.github.io/hce-docs-staging/user-guide/installation/kubernetes/placeholder/). 
- Learn about [Chaos Delegates](https://ksatchit.github.io/hce-docs-staging/technical-reference/content/placeholder/) 

### Prerequisites: 

- Download [chaosctl](https://docs.harness.io/article/35t1zwd5s5-chaos-ctl-reference#downloads), a CLI tool to aid in user authentication and installation of the Chaos Delegate in the Kubernetes cluster 
- Ensure availability of kubectl (for the linux/mac user executing chaosctl). The CLI uses kubectl under the hood to apply manifests.
- Ensure access to the target Kubernetes cluster for chaosctl. The CLI currently uses the default path of kubeconfig, ~/.kube/config.
- Generate and download your `access-key,access-id` pair from ChaosCenter. They will be needed to authenticate the chaosctl account before connecting the Chaos Delegate. 

  - You can generate it from the settings page on ChaosCenter
  
    <img width="1368" alt="Screenshot 2022-09-26 at 11 21 18 AM" src="https://user-images.githubusercontent.com/21166217/192203229-619e6fe9-a489-477a-91eb-e7e7971fecb2.png">

## Steps

#### Step-1: Install chaosctl 

##### Linux/MacOS

- Extract the binary:

  ```
  tar -zxvf chaosctl-<OS>-<ARCH>-<VERSION>.tar.gz
  ```
  
- Provide necessary permissions: 

  ```
  chmod +x chaosctl
  ```
  
- Move the chaosctl binary to /usr/local/bin/chaosctl.

  **Note**: Make sure to use root user or use sudo as a prefix. 

  ```
  mv chaosctl /usr/local/bin/chaosctl
  ```

- Run the chaosctl command:

  ```
  chaosctl <command> <subcommand> <subcommand> [options and parameters]
  ```  
  
##### Windows

- Extract the binary from the zip file using any extraction tool.

- Run the chaosctl command in Windows:

  ``` 
  chaosctl.exe <command> <subcommand> <subcommand> [options and parameters]
  ```
  
- Check the version of the chaosctl:

  ```
  chaosctl version
  ```
  
#### Step-2: Setup the chaosctl account 
  
- Setup a local account with chaoctl

  ```
  chaosctl config set-account --endpoint="" --access_id="" --access_key=""
  ```
  
  Where, `endpoint` corresponds to the ChaosCenter URL
  
#### Step-3: Identify the Chaos Project 

- Identify the project associated with the user account, to which the Chaos Delegate should be connected. 

  To get a list of projects, execute: 
  
  ```
  chaosctl get projects
  ```
  
  You can also obtain the `project id` by copying it from the ChaosCenter: 
  
  <img width="1659" alt="Screenshot 2022-09-26 at 11 30 27 AM" src="https://user-images.githubusercontent.com/21166217/192204074-88f4628d-dc35-47d4-92ab-43882e0cf974.png">
  
#### Step-4: Connect the Chaos Delegate

The chaos delegate can be connected with **cluster scope**, which enables it to discover and inject chaos against workloads across namespaces OR with **namespace scope** which enables it to operate against only those workloads co-residing in the namespace where it is installed.

##### Cluster Scope

- Execute the command: 
  
  ```
  chaosctl connect chaos-delegate --name="" --project-id="" --non-interactive
  ```
  
  This command generates and applies the manifest to install the chaos custom resource definitions (CRDs), deploy the Chaos Delegate (also termed subscriber) along with a few other resources that support the execution of chaos scenarios. 
  
  **Note**: Ensure that the user associated with the current kubernetes context has admin level privileges. This is necessary as CRDs, ClusterRoles & ClusterRoleBindings are installed as part of this exercise. 
  
##### Namespace Scope

- Ensure the following tasks are completed before attempting the Chaos Delegate setup: 

  - Chaos [CRDs](https://github.com/chaosnative/hce-charts/blob/main/k8s-manifests/ci/hce-crds.yaml) are pre-installed by the admin persona before attempting the namespace-scoped delegate connect. 
  
  - The namespace for Chaos Delegate is precreated. Use the command `kubectl create ns <namespace_name>`. This namespace name is required to be passed to the `--namespace` flag in the next step.

- Execute the command: 

  ```
  chaosctl connect chaos-delegate --name="" --project-id="" --non-interactive --installation-mode="namespace" --namespace="" 
  ```

You can learn more about other supported flags for chaosctl [here](https://github.com/chaosnative/chaosctl/blob/main/Usage.md). The CLI tool also supports an [interactive](https://github.com/chaosnative/chaosctl/blob/main/Usage_interactive.md) mode of operation. 

#### Step-5: Verify Chaos Delegate Connectivity

- Verify whether all pods in the Chaos Delegate namespace are running successfully. 

- On sucessful connection, the respective Chaos Delegate entry will be listed as **Active** on ChaosCenter. You can check this by clicking **Chaos Delegates** on the left hand navigation bar. 

  **Note**: Typically, it takes a few seconds for the Chaos Delegate to turn `active` from `pending state`. 

 ## Summary
 
 - Chaos Delegates can be connected to the HCE control plane using the chaosctl CLI tool
 - Chaos Delegates supports a powerful cluster-scoped mode as well as a restrictive namespace-scoped mode of execution 

## Next Steps 

- Create your own chaos scenarios by picking the desired experiments from the ChaosHub 
