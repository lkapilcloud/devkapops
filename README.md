# devkapops
for-kubernetes-autodeploy
**Note: for starting the project please pull this reposity and switch to main branch**

#### Setup minikube cluster with all requirements ########
Assuming you have linux server (in my case amazon linux)

### Step 1: Install docker engine ###### 
sudo yum update -y
sudo yum install docker

### Step 2 : Install Kubectl  
Kubectl is a utility to manage a K8 cluster. Hence it is required to install it before you configure / install the K8 cluster.
Run commands:
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl

**for checking installlation is complete or not**
kubectl version --client 


Once the Docker installation is completed, use the following link to install Kubectl on it:
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/?source=post_page-----e845337a956--------------------------------

### Step 3: install minikube ###  
run command as follows 
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

##  start minikube ##
minikube start


### Step 4: install helm ###  
Run commands as follows:
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod +x get_helm.sh
./get_helm.sh

##  Validate helm installation by executing the helm command. ##
helm 


##  Step 5: install cert-manager executing the helm command. ##
run commands as folllows:
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.13.2 \
  --set installCRDs=true

## enable nginx-ingress within your cluster ##
To enable the NGINX Ingress controller, run the following command:
minikube addons enable ingress

Verify that the NGINX Ingress controller is running:
kubectl get pods -n ingress-nginx

## now build, tag and push the docker image using Dockerfile ##
run following commands:
docker build -t docker-user-name/myapp:latest .
docker tag docker-user-name/myapp:latest
docker push docker-user-name/myapp:latest

## Now run deployment on kubernetes cluster ##
run commands:
kubectl apply -f deployment.yaml
















