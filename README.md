# azure-bootcamp-aks-selenium-grid

# Create AKS Cluster using Azure-CLI
Open powershell or whatever terminal you prefer, login to Azure: 

$ az login

# Create Resource Group with location

$ az group create -g Selenium -l "eastus"

# Create cluster

$ az aks create -g Selenium -n k8s-grid

#To Create cluster in Azure Cloud Shell 

$ az aks create -g Selenium -n k8s-grid --generate-ssh-keys

# Now connect to the cluster specifying the Resource Group and Cluster name we gave in a previous step:

$ az aks get-credentials --resource-group Selenium --name k8s-grid

# You should see something like 
Merged "k8s-grid" as current context in C:\Users\xxx\.kube\config
That’s it, you are connected!

# Create the Hub
Save selenium-hub-deployment.yaml in local folder, then run :

$ kubectl create -f selenium-hub-deployment.yaml

# Now see the pod initializing/ running

$ kubectl get pods 

# Create a service to allow us to get to the Hub web interface, note we need to make sure the type is set to ‘LoadBalancer‘ and create selenium-hub-svc.yaml:
To deploy:  

$ kubectl create -f selenium-hub-svc.yaml

Now run:

$ kubectl get services

# Create the Nodes

Chrome
kubectl create -f .\selenium-node-chrome-deployment.yaml 

Firefox
kubectl create -f .\selenium-node-firefox-deployment.yaml 

# To check the Pods
$ kubectl get pods

# Scaling the Grid
We can scale by replicating nodes and have them sit idle:

$ kubectl get deployments

$ kubectl scale deployment selenium-node-firefox --replicas=10

# To AutoScale :

$ kubectl autoscale deployment selenium-node-chrome --cpu-percent=50 --min=1 --max=10
