summary: POC Sandbox
id: danatrace.github.io
categories: Sandbox, EKS, 
tags: POC-Sandbox
status: Published
authors: daniel.braaf

## Welcome to the POC Sandbox
Duration: 1

Welcome to the Dynatrace POC-Sandbox.
The POC-Sandbox is a hands on environment meant to demonstrate the ease of use of the Dynatrace Platform. 
The POC-Sandbox infrastructure consists of
* a Kubernetes Cluster with 3 Nodes
* a Shell Instance that can connect to the cluster and perform kubectl commands
* Example apps that can be deployed to the cluster 


## Connect to POC-Sandbox Shell Instance
Duration: 5
#### Download the key-pair file that is attached to the Sandbox creation completion email

<table style="width:100%;">
  <tr>
  <td><img align="center" src="assets/downloadkeypair.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>



#### Save the keyfile to a folder of your choice

<br>
<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/saveas.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>
<br>




#### Copy the ssh command from the Sandbox creation completion email

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/copycmd1.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Open a Command Line prompt and access the directory where the key was downloaded to
#### Paste the ssh command from the Sandbox creation completion email
```bash
C:\Users\user\Downloads> ssh -i {yourpemkeyname}.pem ubuntu@{yourinstancedns}.com
```

#### You have successfully connected to the shell instance with access to your POC-Sanbdox Cluster

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/shellinstance.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

## Connect shell instance to POC-Sanbox K8s Cluster


#### Copy the 2nd command from the Sandbox creation completion email

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/copycmd2.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Paste the second command into your shell instance
```bash
aws eks --region us-east-1 update-kubeconfig --name {your-tenant-id}-sandbox
```

#### Test if the connection was successful with the following command 
```bash
kubectl get namespaces
```

#### Your output should look like this

```bash
NAME              STATUS   AGE
default           Active   5h8m
kube-node-lease   Active   5h8m
kube-public       Active   5h8m
kube-system       Active   5h8m
```

## Connect Dynatrace with the POC-Sandbox (K8s-CLuster)

#### In the Dynatrace UI Navigate to Deploy Agent -> Kubernetes

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/installagent1.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Provide a name, click the 2 "Create Token" buttons, click the Download dynakube.yaml button

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/installagent2.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Copy the content of the downloaded dynakube.yaml

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/copyyaml.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Go back to the Shell Instance, create a new file with the name dynakube.yaml

```bash
nano dynakube.yaml
```

#### Copy the content of the downloaded dynakube.yaml into the newly created dynakube.yaml in the shell instance and save the file

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/copiedyaml.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Copy the command section from the Install Dynatrace overview

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/copycmd3.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Paste and execute the copied commands into the shell instance

```bash
kubectl create namespace dynatrace
kubectl apply -f https://github.com/Dynatrace/dynatrace-operator/releases/download/v0.11.1/kubernetes.yaml
kubectl -n dynatrace wait pod --for=condition=ready --selector=app.kubernetes.io/name=dynatrace-operator,app.kubernetes.io/component=webhook --timeout=300s
kubectl apply -f dynakube.yaml
```

#### You should get the following output in the shell instance

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/connectioncreated.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Click on "Show deployment Status" in the Install Kubernetes overview in Dynatrace

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/showdeployment.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### The Agents will be displayed as soon as they make a connection 

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/showagents.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

## Deploy Easytravel 

#### In the shell instance cd into the easytravel-k8s directory 

```bash
cd /home/ubuntu/poc-sandbox/apps/easytravel-k8s
```

#### Run the script deploy_easytravel.sh and wait until the script has completed
```bash
~/poc-sandbox/apps/easytravel-k8s$ ./deploy-easytravel.sh
```

#### When the script has completed it will output the url the loadbalancer 

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/loadbalancer.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Copy the loadbalancer url and paste it into your browser

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/applicationloadbalancer.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### you have successfully deployed the easytravel app 

#### to remove the easytravel app run remove-easytravel.sh
```bash
~/poc-sandbox/apps/easytravel-k8s$ ./remove-easytravel.sh
```

#### this is the expected output after removal of the easytravel application

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/removeeasytravel.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>


## Deploy Dynabank 

#### In the shell instance cd into the directory 

```bash
cd /home/ubuntu/poc-sandbox/apps/dynabank
```

#### Run the script deploy_dynabank.sh and wait until the script has completed
```bash
~/poc-sandbox/apps/dynabank$ ./deploy-dynabank.sh
```

#### When the script has completed it will output the loadbalancer url

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/deploydynabank.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Copy the Loadbalancer url and paste it into your browser

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/dynabankapp.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### you have successfully deployed the dynabank app 

#### to remove the dynabank app and loadbalancer execute the remove-dynabank.sh
```bash
~/poc-sandbox/apps/dynabank$ ./remove-dynabank.sh
```

#### this is the expected output after removal of the dynabank application

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/dynabankremove.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>


## Deploy Bobbleneers 

#### In the shell instance cd into the directory 

```bash
cd /home/ubuntu/poc-sandbox/apps/bobbleneers
```

#### Run the file deploy_bobbleneers.sh and wait until the script has completed
```bash
~/poc-sandbox/apps/bobbleneers$ ./deploy-bobbleneers.sh
```

#### When the script has completed it will output the following 

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/deploybobbleneers.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>


#### you have successfully deployed the bobbleneers app 

#### to remove the Bobbleneers app and loadbalancer run remove-bobbleneers.sh
```bash
~/poc-sandbox/apps/bobbleneers$ ./remove-bobbleneers.sh
```

#### this is the expected output after removal of the bobbleneers application

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/bobbleneersremove.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

## Deploy Simplcommerce 

#### In the shell instance cd into the directory 

```bash
cd /home/ubuntu/poc-sandbox/apps/simplcommerce
```

#### Run the script deploy_simplcommerce.sh and wait until the script has completed
```bash
~/poc-sandbox/apps/simplcommerce$ ./deploy-simplcommerce.sh
```

#### When the script has completed it will output the loadbalancer url

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/deploysimplcommerce.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### Copy the Loadbalancer url and paste it into your browser

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/simplcommerceapp.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>

#### you have successfully deployed the simplcommerce app 

#### to remove the simplcommerce app and loadbalancer run remove-simplcommerce.sh
```bash
~/poc-sandbox/apps/simplcommerce$ ./remove-simplcommerce.sh
```

#### this is the expected output after removal of the simplcommerce application

<table style="width:100%;">
  <tr>
 <td><img align="center" src="assets/dynabankremove.png" alt="Download Keypair" width="700" height="700" ><br><br></td>
  </tr>
</table>
