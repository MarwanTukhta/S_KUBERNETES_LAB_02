1- Deploy a pod named nginx-pod using the nginx:alpine image with the labels set to   tier=backend.
<br>
check file `nginx-alpine-pod.yaml`

2- Deploy a test pod using the nginx:alpine image.
<br>
check file `nginx-test-pod.yaml`

3- Create a service backend-service to expose the backend application within the cluster on port 80.
<br>
check file `nginx-backend-service.yaml`

4- try to curl the backend-service from the test pod. What is the response?
```
/ # curl 172.17.0.16:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

5- Create a deployment named web-app using the image nginx with 2 replicas
<br>
check file `web-app-deployment.yaml`

6- Expose the web-app as service web-app-service application on port 80 and nodeport 30082 on the nodes on the cluster
<br>

check file `web-app-service.yaml`

7- access the web app from the node

```
root@minikube:/# curl 10.97.74.15:80   
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

8- How many Nodes exist on the system?

```
PS C:\Users\imarw\Desktop\k8s_practace\S_KUBERNETES_LAB_02> kubectl get node 
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   2d14h   v1.23.3
```

9- Do you see any taints on master ?
<br>

no

10- Apply a label color=blue to the master node

```
PS C:\Users\imarw\Desktop\k8s_practace\S_KUBERNETES_LAB_02> kubectl label nodes minikube color=blue
node/minikube labeled
```

11- Create a new deployment named blue with the nginx image and 3 replicas
<br>
     Set Node Affinity to the deployment to place the pods on master only<br>
     NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution<br>
     Key: color<br>
     values: blue
<br>

check file `blue-deployment.yaml`


12- How many DaemonSets are created in the cluster in all namespaces?

```
PS C:\Users\imarw\Desktop\k8s_practace\S_KUBERNETES_LAB_02> kubectl get DaemonSets --all-namespaces
NAMESPACE     NAME         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE        
kube-system   kube-proxy   1         1         1       1            1           kubernetes.io/os=linux   2d14h      
``` 

13- what DaemonSets exist on the kube-system namespace?
<br>
kube-proxy

14- What is the image used by the POD deployed by the kube-proxy DaemonSet
<br>
k8s.gcr.io/kube-proxy:v1.23.3

15- Deploy a DaemonSet for FluentD Logging. Use the given specifications.
<br>
      Name: elasticsearch<br>
      Namespace: kube-system<br>
      Image: k8s.gcr.io/fluentd-elasticsearch:1.20
<br>
check file `FluentD-DaemonSet.yaml`

16- Create a taint on node01 with key of spray, value of mortein and effect of NoSchedule

```
PS C:\Users\imarw\Desktop\k8s_practace\S_KUBERNETES_LAB_02> kubectl taint nodes minikube spray=mortein:NoSchedule   
node/minikube tainted
```

17- Create a new pod named mosquito with the NGINX image

```
PS C:\Users\imarw\Desktop\k8s_practace\S_KUBERNETES_LAB_02> kubectl run mosquito --image=nginx
pod/mosquito created
```

18- What is the state of the mosquito POD?
<br>
Pending

19- Create another pod named bee with the NGINX image, which has a toleration set to the taint Mortein
<br>
    Image name: nginx<br>
	Key: spray<br>
	Value: mortein<br>
	Effect: NoSchedule<br>
	Status: Running
<br>

check file `bee-pod.yaml`

20- Remove the taint on master/controlplane, which currently has the taint effect of NoSchedule
```
PS C:\Users\imarw\Desktop\k8s_practace\S_KUBERNETES_LAB_02> kubectl taint nodes minikube spray=mortein:NoSchedule-  
node/minikube untainted
```

21- What is the state of the pod mosquito now and Which node is the POD mosquito on?
<br>
STATUS: running<br>
node: minikube

22- Create a job countdown-job.<br>
The container should be named as container-countdown-job<br>
Use image debian:latest, and restart policy should be Never.<br>
Use command for i in ten nine eight seven six five four three two one ; do echo $i ; done<br>

check file `countdown-job.yaml`

<br>
output:
```
PS C:\Users\imarw\Desktop\k8s_practace\S_KUBERNETES_LAB_02> kubectl logs countdown-job-m9qql
ten
nine
eight
seven
six
five
four
three
two
one
```