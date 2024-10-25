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
  - name: echoserver:2.2
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
Включил port-forward:
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

1. Создать Pod с именем netology-web.
2. Использовать image — gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
3. Создать Service с именем netology-svc и подключить к netology-web.
4. Подключиться локально к Service с помощью `kubectl port-forward` и вывести значение (curl или в браузере).
