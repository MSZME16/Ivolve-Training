# Apache Deployment on OpenShift with Rollback to Nginx

This guide will walk you through the steps to deploy an Nginx application on OpenShift, expose it as a service, access it locally, update the deployment to Apache, observe the pod lifecycle, and rollback to the Nginx deployment.

## Prerequisites

- Access to an OpenShift cluster
- OpenShift CLI (`oc`) installed and configured

## Steps

### 1. Log in to Your OpenShift Cluster

```sh
oc login <your-cluster-url> --username=<your-username> --password=<your-password>
```
### 2. Create a New Project (Optional)

```sh
oc new-project my-web-project
```
### 3. Create the Nginx Deployment

```sh
oc apply -f deployment.yml
```
![alt text](screenshots/deployment.png)

```sh
oc expose deployment deployment --port=80 --target-port=80
```

![alt text](screenshots/svc-route.png)

### 4. Verify the Service Details

```sh
oc get svc
```
![alt text](screenshots/svc-route.png)

### 6. Access the Service Locally

Forward a local port to the Nginx service:

```sh
oc port-forward svc/nginx-deployment 8080:80
```

Access the service locally via `http://localhost:8080` and take a screenshot of the Nginx welcome page.

![alt text](screenshots/nginx.png)

### 7. Update the Deployment to Use Apache

```sh
oc set image deployment/nginx-deployment nginx=httpd
```

### 8. Watch the Status of the Pods

```sh
oc get pods -w
```

![Take a screenshot of the pod status showing the transition from Nginx to Apache.](screenshots/change.png)

Access the service locally via `http://localhost:8080` and take a screenshot of the Nginx welcome page.

![alt text](screenshots/apache.png)

### 9. Show the Rollout History

```sh
oc rollout history deployment/nginx-deployment
```

![alt text](screenshots/rollout1.png)
![alt text](screenshots/rollout2.png)

### 10. Rollback to the Previous Nginx Deployment

```sh
oc rollout undo deployment/nginx-deployment to-revision=1
```

![alt text](screenshots/rollback.png)

### 11. Watch the Status of the Pods During Rollback

```sh
oc get pods -w
```
![alt text](screenshots/full-pod-change.png)

![alt text](screenshots/nginx.png)

