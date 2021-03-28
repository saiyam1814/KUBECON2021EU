# KUBECON2021EU
This Repo is for the demo for running Chaos experiment on Civo Kubernetes.

Create a K3s cluster (In this Demo we are using Civo Kubernetes cluster). You can also create K3s cluster on Katakoda ubuntu playgound or any other VM instance. 

Steps:
- Signup for Civo (https://www.civo.com/signup)
- Download the Civo cli (https://github.com/civo/cli)
- Configure the cli with api key (https://github.com/civo/cli#api-keys)
- create cluster using below command
`civo kubernetes  create chaos`
- Install Litmus 2.0 
`
git clone https://github.com/litmuschaos/litmus-helm
cd litmus-helm
helm install litmuschaos  --namespace litmus ./charts/litmus-2-0-0-beta/

NAME: litmuschaos
LAST DEPLOYED: Sat Mar 27 17:40:34 2021
NAMESPACE: litmus
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing litmus-2-0-0-beta 😀

Your release is named litmuschaos and it's installed to namespace: litmus.
`
- Check the installation 

`
kubectl get pods -n litmus

NAME                                                      READY   STATUS    RESTARTS   AGE
litmuschaos-litmus-2-0-0-beta-frontend-6bbdb89479-lvftk   1/1     Running   0          24h
litmuschaos-litmus-2-0-0-beta-server-5fdf755679-qnvzj     2/2     Running   0          24h
litmuschaos-litmus-2-0-0-beta-mongo-0                    1/1     Running   0          24h

`

- 
