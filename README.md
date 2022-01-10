# Kubernetes project on GCP

### 1. Deploying the containerized ML model to Kubernetes
#### -- Prepare the structure of this project that you create is as follows:

| api.py <br/>
| base <br/>
      | namespace.yaml <br/>
      | deployment.yaml <br/>
      | service.yaml <br/>
      | kustomization.yaml <br/>
| Dockerfile <br/>

#### -- Install Kustomize by running below command lines
    ##### -- Download Kustomize
    docker pull k8s.gcr.io/kustomize/kustomize:v3.8.7
    docker run k8s.gcr.io/kustomize/kustomize:v3.8.7 version
    ##### -- Unzip downloaded Kustomize file -> kustomize and move kustomize to the primary directory of executable commands on the system. (Note: /usr/bin is a standard directory on Unix-like operating systems that contains most of the executable files (i.e., ready-to-run programs) that are not needed for booting (i.e., starting) or repairing the system.)
    tar xzf ./kustomize_v*_linux_amd64.tar.gz && \
    mv kustomize /usr/bin/
