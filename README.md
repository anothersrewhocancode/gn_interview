I have not used the Github  [Ubuntu Runner](https://github.com/actions/runner-images/blob/main/images/linux/Ubuntu2204-Readme.md) as there was'nt enough room for the resources to spin up a 2 node Kubernetes Cluster.
I have spun up a ec2 instance and then added the ec2 instance as a self-hosted runner.

### Kubernetes Cluster Configuration For Kind
```
kind: ClusterConfiguration
# configure controller-manager bind address
controllerManager:
  extraArgs:
    bind-address: 0.0.0.0 #Disable localhost binding
    secure-port: "0"      #Disable the https
    port: "10257"         #Enable http on port 10257
# configure etcd metrics listen address
etcd:
  local:
    extraArgs:
      listen-metrics-urls: http://0.0.0.0:2381
# configure scheduler bind address
scheduler:
  extraArgs:
    bind-address: 0.0.0.0  #Disable localhost binding
    secure-port: "0"       #Disable the https
    port: "10259"          #Enable http on port 10259
```
Create the cluster using the below command

`$ kind create cluster --name mycluster --config cluster.yaml`

I have also installed other binaries needed for Deploying Kubernetes objects like
- kubectl
- helm
- apache2-utils

Load Testing Setup:
ab tool ( Apache Bench)  is used to generate traffic for the load testing.

The helm charts also has Pod disruption budgets to prevent voluntary eviction.
