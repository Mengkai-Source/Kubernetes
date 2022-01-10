# Kubernetes project on GCP

### 1. Containerizing a simple ML model scoring service using Flask and Docker

#### Defining the Docker image with the Dockerfile
docker build -t ml-k8s .

#### Pushing the Docker Image to Container Registry
docker logout
docker login -u bosmk -p [PASSWORD]
docker tag ml-k8s bosmk/ml-k8s
docker push bosmk/ml-k8s

### 2. Setting up and connecting to the Kubernetes cluster

Ensure that you have enabled the Google Kubernetes Engine API. You can enable an API in the Cloud Console.

Start a cluster: \
$ gcloud container clusters create k8s-ml-cluster --num-nodes 3 --machine-type g1-small --zone us-west1-b \
You may need to wait a moment for the cluster to be created.

Connect to the cluster: \
$ gcloud container clusters get-credentials tf-gke-k8s --zone us-west1-b --project [PROJECT_ID]


### 3. Deploying the containerized ML model to Kubernetes
#### -- Prepare the structure of this project that you create is as follows:

| api.py \
| base\
      -- | namespace.yaml\
      -- | deployment.yaml\
      -- | service.yaml\
      -- | kustomization.yaml\
| Dockerfile

#### -- Install Kustomize by running below command lines
    ##### -- Download Kustomize
    docker pull k8s.gcr.io/kustomize/kustomize:v3.8.7
    docker run k8s.gcr.io/kustomize/kustomize:v3.8.7 version
    ##### -- Unzip downloaded Kustomize file -> kustomize and move kustomize to the primary directory of executable commands on the system. (Note: /usr/bin is a standard directory on Unix-like operating systems that contains most of the executable files (i.e., ready-to-run programs) that are not needed for booting (i.e., starting) or repairing the system.)
    tar xzf ./kustomize_v*_linux_amd64.tar.gz && \
    mv kustomize /usr/bin/
    
#### Deploy the app
After setting up these YAML files, you can deploy your app using this single command: \
kubectl apply --kustomize=${PWD}/base/ --record=true

To see all components deployed into this namespace use this command: \
$ kubectl get ns

-- Example: \
      NAME              STATUS   AGE \
      default           Active   69m \
      kube-node-lease   Active   69m \
      kube-public       Active   69m \
      kube-system       Active   69m \
      mlops             Active   27m
      
To see the status of the deployment, use this command: \
$ kubectl get deployment -n mlops

-- Example: \
      NAME        READY   UP-TO-DATE   AVAILABLE   AGE
      score-app   2/2     2            2           28m
      
To see the status of the service, use this command: \
$ kubectl get service -n mlops

-- Example: \
      NAME        TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)          AGE
      score-app   LoadBalancer   xx.x.xx.xxx   xx.xxx.xxx.xx   5000:xxxxx/TCP   29m
