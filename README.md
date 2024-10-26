# Домашнее задание к занятию «Базовые объекты K8S»
### Задание 1. Создать Pod с именем hello-world

Создал манифест echo.yml:

```yml
apiVersion: v1
kind: Pod
metadata:
  name: hello world
spec:
  containers:
  - name: echoserver
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```
Cоздал pod:
```bash
user@microk8s:~$ kubectl apply -f echo.yml
pod/hello-world created
user@microk8s:~$ kubectl get pods
NAME          READY   STATUS    RESTARTS   AGE
hello-world   1/1     Running   0          4m48s
```
Включил port-forward на pod:
```bash
user@microk8s:~$ kubectl port-forward pod/hello-world 8080:8080
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```
Получил вывод curl:
```bash
user@microk8s:~$ curl 127.0.0.1:8080


Hostname: hello-world

Pod Information:
        -no pod information available-

Server values:
        server_version=nginx: 1.12.2 - lua: 10010

Request Information:
        client_address=127.0.0.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://127.0.0.1:8080/

Request Headers:
        accept=*/*
        host=127.0.0.1:8080
        user-agent=curl/8.5.0

Request Body:
        -no body in request-
```


------

### Задание 2. Создать Service и подключить его к Pod

Создал манифест netology-web.yml:
```yml
apiVersion: v1
kind: Pod
metadata:
  name: netology-web
  labels: 
    app: netology-web
spec:
  containers:
  - name: echoserver
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080

```
Создал манифест netology-svc.yml:
```yml
apiVersion: v1
kind: Service
metadata:
  name: netology-svc
spec:
  selector:
    app: netology-web
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080

```
Создал pod:
```bash
user@microk8s:~$ kubectl apply -f netology-web.yml 
pod/netology-web created
```
Создал service:
```bash
user@microk8s:~$ kubectl apply -f netology-svc.yml 
service/netology-svc created
```
```bash
user@microk8s:~$ kubectl get pod
NAME           READY   STATUS    RESTARTS      AGE
netology-web   1/1     Running   1 (24m ago)   41m
user@microk8s:~$ kubectl get service
NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes     ClusterIP   10.152.183.1    <none>        443/TCP    2d1h
netology-svc   ClusterIP   10.152.183.55   <none>        8080/TCP   39m
```
Включил port-forward на service:
```bash
user@microk8s:~$ kubectl port-forward svc/netology-svc 8080:8080
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```
Получил вывод curl:
```bash
user@microk8s:~$ curl 127.0.0.1:8080

Hostname: netology-web

Pod Information:
        -no pod information available-

Server values:
        server_version=nginx: 1.12.2 - lua: 10010

Request Information:
        client_address=127.0.0.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://127.0.0.1:8080/

Request Headers:
        accept=*/*  
        host=127.0.0.1:8080  
        user-agent=curl/8.5.0  

Request Body:
        -no body in request-
```
