## Tools You Need to Install:
* Docker: 

* Minikube:

* Kubectl: 

* Helm: 

* Git:


## Steps to Run the Project Locally:

### 1.  Start Minikube:

```bash
minikube start --driver=docker

```

### 2. Enable Ingress for Minikube:

``` bash
minikube addons enable ingress
```
### 3. Set up the Kubernetes environment variables:
```bash
kubectl config use-context minikube
```

### 4. Build the Docker image for the website:
```bash
docker build -t tributech-website:local .
```

### 5. Load the Docker image into Minikube:
```bash
minikube image load tributech-website:local
```


### 6. Deploy the application using Helm:
```bash
helm upgrade --install tributech ./tributech -f tributech/values.yaml
```


### 7. Patch the ingress service to LoadBalancer type (it is not set automatically):
```bash
kubectl patch svc ingress-nginx-controller -n ingress-nginx -p '{"spec": {"type": "LoadBalancer"}}'
```


### 8. Check the status of the pods:
```bash
kubectl get pods -A
```


### 9. Monitor the logs of the website pod:
```bash
kubectl logs -f <website_pod_name>
```


### 10. Access the website by navigating to the provided IP and port:
```bash
minikube service tributech-website
```