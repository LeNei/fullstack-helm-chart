# api-helm-chart
## Hetzner Setup
Setup cloud instance and firewall with the following ports 22, 80 and 443.
<br/>
<br/>
After that connect to the instance over ssh and in update + upgrade everything:
<br/>
`apt update -y && apt upgrade -y`
<br/>
<br/>
To install k3s run the following command:
<br/>
`curl -sfL https://get.k3s.io | sh -`
<br/>
<br/>
To get the kube config execute the following command:
<br/>
`scp root@remoteHost:/etc/rancher/k3s/k3s.yaml ~/.kube/k3s-config`

## Accessing The Cluster
To get acces to the cluster make a safety copy of your current `~/.kube/config` and rename the copied config from your setup.
After that you have to forward the kubernetes API-Port from your server to your local machine:
<br/>
`ssh -L 6443:remoteHost:6443 root@remoteHost`
<br/>
Now you should be able to run `kubectl` commands on your cluster.

## cert-manager Setup
Since cert-manager is part of our helm chart we have to make sure the crd is installed beforehand so we have access to all resources. We do that with the following command:
<br/>
`kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.1.1/cert-manager.crds.yaml`

## Starting the chart
If we want to run the chart in its own namespace we can do so by creating one and switching to it. Here we create the namespace `prod`:
<br/>
`kubectl create namespace prod && kubectl config set-context --current --namespace=prod`
<br/>
We should also make sure that our DNS is pointing to the correct IP so cert-manager can fetch the certificate without a problem.
Now we can just run our chart and we should be good to go :)
