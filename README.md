# 1. Full Stack Development
Diary of full stack software development

# 2. Introduction
There is need to document the entire process of software devlopment with practical steps which are easy to follow for others to learn from. This documentation provides very specific steps on projects management, identification of requirements, software development life cycle, tools and deployment all best on enterprise and best industry practices. The first version of this document pays particular attention to software development - tools, langauges deployment thoughts in form of my go to manual of documented steps on how to achieve particular objectives or go around a nuggy problem in software development and design. ALso some decison choices over convention in some cases.

# 3. Project Management

# 4. Full Stack
I use full stuck meaning from deisgn and developemnt - front end (View), middledware (Control), and backend (Model/Data) using microservices to decouple these design concerns to deployment considering aspects of containerising and software orchestration (automation of depoyment/management) using docker and kubernetes. Further this is looks at continous improving of software and applying the patches with minimals disruption of the system.
### Design and development Best Practices

# 5. Tools and Languages

# 6. Front End - View

In designing the client, Ia am looking at having a client flexiibility to be ugraded an improved on without looking too much about the middle ware and the data. The tools I have decided to use is flutter. I can build mobile, web and desktop applications from the same code base.

## Flutter - Mobile
### Installation
### Creating a project

```console
~ flutter create [folder_name]
```
**name should have underscore according to flutter**

```console
~ mkdir [folder_name] && cd [folder_name] && flutter create .
```
### Running flutter application
You can check what devices are available and running first:
```console
~ flutter devices
2 connected devices:

Android SDK built for x86 (mobile) • emulator-5554 • android-x86    • Android 10 (API 29) (emulator)
Chrome (web)                       • chrome        • web-javascript • Google Chrome 89.0.4389.90
```
Choosing the device then run in the root of application/project
```console
flutter run -d chrome
```
clean the flutter project due to unknwon errors 
```console
~ flutter clean
```
after changes pubspec.yaml file or you do a clean like in above run the update dependencies command to fetch them where ever they may be sitting
```console
~ flutter pub get
```
## How to use the runnable command to generate "part" files especially when using the json_serializable module
```console
~ flutter pub run build_runner build
```
## Null Safety - Dart
sometimes if there is no compatible code add the switch
```console
--no-sound-null-safety
```
as in the ff example
```console
~ flutter run -d chrome --no-sound-null-safet
```
# 6. Middleware - Controller
I chose java and spring-boot to build the middleware. 
# 7. Data - Model

# 8. Overal architectural design and decision

# 9. Microsservices
## Why microservices
## Seperation of business concern
# 10. Authentication

# 11. Versioning

# 12. Git/GitHut

# 13. Deployment and Maintenance
## Deplyment
Docker and kubernetes are for packaging, automating and managing the completed system.
## Java Deployment
After design and testing a microservice it can be deployment to its happily thereafter home in the cloud. An image needs to be created for it and uploaded to the [docker hub](http://hub.docker.com) an account is required.

# 14. Docker
## Why docker
## Installation
More details on installation of docker [here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)
## Running docker as user
1. add your current user to docker group
```console
~ sudo usermod -aG docker ${USER}
```
2. Check which groups you belong to and whether you have been added to the docker group
```
~ id -nG
teddy adm cdrom sudo dip plugdev lxd microk8s docker
```
3. Load the user
```console
~ su - ${USER
```
## Common docker commands 
## Deploying Java to production using docker
1. Packaging
```console
~ mvn spring-boot:build-image -DskipTest
```
2. Pushing to hub dot docker
```console
~ docker push lubasi/fapp1-farm-authentication:0.0.1-SNAPSHOT
```
3. pulling to production environment
```console
~ docker pull lubasi/fapp1-farm-authentication:0.0.1-SNAPSHOT
```
Running this deployment refer to kubernetes section
# 15. Kubernetes - microk8s
Kube thats the direction computing is taking in the cloud. Its to make the cloud easier to work with when deploying apps/systems
## Why kubernetes
## Installation
More eleborate steps [here](https://ubuntu.com/tutorials/install-a-local-kubernetes-with-microk8s#2-deploying-microk8s)
## Kubernetes Dashboard
Once installation of kube is done it was not clear in the documentation above on how to access the dashboard. The kubernetes dashboard is the web-based management too for bubernete infrastreucture, the whole stuff packed in kubectl can be found in the dashboard...giving a visual outlook of the kube infrustructure. Quick acces to the dashboard run the proxy:
```console
~ microk8s dashboard-proxy
Checking if Dashboard is running.
Dashboard will be available at https://127.0.0.1:10443
Use the following token to login:
eyJhbGciOiJSUzI1NiIsImtpZCI6ImtJYnd0ZEp0akJiRDlVNXRLNFd2SVNiRS1RZF9uaGQ1R2tNZ0E1Qm9KY1EifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLXhzbjRtIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiI1NTM2OGRlMi01MjBmLTQ4NDMtYTlmZi0xNWM5OWVlMjY0ZWEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.UjytxDCvYOxCLjnlGwXgrUCeGTCv7ie3zW1tvnshfDGD6FrvVsbuxX2qXnJio2Q1pgkDB8MNhf4fTiD3MjeSplXg00-7um_MRmuEk9bRzJhEOXNpt76xzW4NEN0ksOZDYFb9nASasEA5d1mzHVji3HEXTqFHXrLgBkn2AFO-6BQuG3VoIuWSspRUFgXQSJUhoHjJ5igARg2P6-y32kNy-ZUGG3Haja3JKfmDQdAwLfihagQkspXWvbW-UUUyuFwb1LlV9jYq4Gn76Uuc66wTDSnL0FrIdl9f2YbQcqtgTub8v2JTeACAGjlPB-Aib4rnRKpm6Bmfs5HBKeXtyQks2Q
```
Gnereates a token. Copy that and head over to the browser on the same machine or point to the IP address of the kube installation. Details of installing own proxy using NGINX can

## Running a deplyment
⋅⋅* Once the image is on hub dot coker it can be deployment to production automatically using kubectl (kubernetes tool for interacting with the kubernetes infrastructure
```console
~ microk8s.kubectl create deployment fapp1 --image=lubasi/fapp1-farm-authentication:0.0.1-SNAPSHOT
```
This command create pod (single unit of deployment constains images, kubelet coordinating the pod, networking capability) to view the view a deployment and pod:

⋅⋅* View deployment created
```console
~ microk8s.kubectl get deployment

NAME       READY   UP-TO-DATE   AVAILABLE   AGE
microbot   3/3     3            3           17h
fapp1      1/1     1            1           12h
```
View pods
```console
~ microk8s.kubectl get pod
NAME                        READY   STATUS    RESTARTS   AGE
microbot-5f5499d479-dpwpp   1/1     Running   0          17h
microbot-5f5499d479-9rr6d   1/1     Running   0          17h
microbot-5f5499d479-pdrnj   1/1     Running   0          14h
fapp1-8676856789-kjkl6      1/1     Running   0          13h
```
To view logs if the pod if it was successful deployed
```console
~ microk8s.kubectl logs -f fapp1-8676856789-kjkl6
```
Scale a deployment to run multiple copies of itself with its own resources
```console
~ microk8s.kubectl scale deployment fapp1 --replicas=3
deployment.apps/fapp1 scaled
```
View scaled up/down deployments
```console
microk8s.kubectl get deployment
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
microbot   3/3     3            3           17h
fapp1      3/3     3            3           13h
```
## Expose a service
Expose service running on kube
```console
~ microk8s.kubectl expose deployment fapp1 --type=NodePort --port 8000 --name=fapp1-service
```
To view the deployed service and view what ip address/port it is using
```console
microk8s.kubectl get all --all-namespaces |grep service

default       service/kubernetes                  ClusterIP   10.152.183.1     <none>        443/TCP                  17d
kube-system   service/kube-dns                    ClusterIP   10.152.183.10    <none>        53/UDP,53/TCP,9153/TCP   17d
kube-system   service/metrics-server              ClusterIP   10.152.183.219   <none>        443/TCP                  17d
kube-system   service/kubernetes-dashboard        ClusterIP   10.152.183.99    <none>        443/TCP                  17d
kube-system   service/dashboard-metrics-scraper   ClusterIP   10.152.183.236   <none>        8000/TCP                 17d
default       service/microbot-service            NodePort    10.152.183.141   <none>        80:31369/TCP             15h
**default       service/fapp1-service               NodePort    10.152.183.50    <none>        8000:31449/TCP           12h**
```
This shows that our deployment is using an internal IP addres (Internals of Kubernetes infrastructure, you cannot even ping it i think ) running inside kube on port 8000 and externally listening on port 31449

## Creating a proxy to access using Nginx
### Installation of Nginix reverse proxy
Details can be found [here](https://linuxize.com/post/nginx-reverse-proxy/)
## Common Kube commands


