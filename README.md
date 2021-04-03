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
```
kubectl create ns litmus 
kubectl apply -f https://litmuschaos.github.io/litmus/2.0.0-Beta/litmus-2.0.0-Beta.yaml

kubectl get pods -n litmus

NAME                                                      READY   STATUS    RESTARTS   AGE
litmuschaos-litmus-2-0-0-beta-frontend-6bbdb89479-lvftk   1/1     Running   0          24h
litmuschaos-litmus-2-0-0-beta-server-5fdf755679-qnvzj     2/2     Running   0          24h
litmuschaos-litmus-2-0-0-beta-mongo-0                    1/1     Running   0          24h
```

- Deploy the application 
```
kubectl apply -f deploy/hello.yaml
kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
helloservice-579fdcf676-2c4f4   1/1     Running   0          10m
```
- Eanble Gitops in Litmus 

![image](https://user-images.githubusercontent.com/8190114/113475830-4d215480-9495-11eb-93d2-5a469d86aa6d.png)

- Deploy Prometheus, Blackbox exporter and Grafana

```
kubectl apply -f monitoring/prometheus
kubectl apply -f monitoring/blackbox-exporter
kubectl apply -f monitoring/grafana
```

- Create Grafana Dashboard by adding Prometheus as the data source and Json data from monitoring/grafana/dashboard.json
![image](https://user-images.githubusercontent.com/8190114/113475883-780ba880-9495-11eb-9661-750e6f063cef.png)

- Create a custom pod networkloss Workflow and set monitoring to true
![image](https://user-images.githubusercontent.com/8190114/113475763-24995a80-9495-11eb-818b-ba73dc892d80.png)


- Bootstrap Flux and use the path as deploy folder to put the yaml for deployment with new image. It is using the Jinja template in the tmpl folder

```
flux bootstrap github --components-extra=image-reflector-controller,image-automation-controller --owner=saiyam1814 --repository=KUBECON2021EU --branch=main --path=deploy --token-auth --personal
```
- Setup Github Actions for build and push image and create manifest in the deploy folder




