# [YouTube, Paul C] Creating a Helm Chart Repository using Github Actions [ENG, 2020]

**The idea is from YouTube Video:**  
https://www.youtube.com/watch?v=hL-8Jn5RTmw

<br/>

**Demo**  
https://github.com/helm/charts-repo-actions-demo

<br/>

## Prepare Repository

    $ git checkout --orphan gh-pages
    $ rm -rf *
    $ rm -rf .gihtub
    $ rm -rf .gitignore
    $ git add --all
    $ git commit -m "empty gh-pages branch"
    $ vi index.html

<br/>

```
<h1>Helm Chart Repo</h1>
```

<br/>

    $ vi CNAME

<br/>

```
k8s-helm-repo.jsdev.org
```

<br/>

    $ git add --all
    $ git commit -m "index html"
    $ git push origin gh-pages

<br/>

### Domain Name DNS Provider

<br/>

CNAME -> k8s-helm-repo.jsdev.org -> github.io

<br/>

![Application](/img/dns-settings.png?raw=true)

<br/>

![Application](/img/create-repo-page.png?raw=true)

<br/>

### Github Account Settings

GitHub -> Settings -> Personal access tokens -> Create Token

<br/>

HELM_CHART_REPOSITIORY_TOKEN

<br/>

GitHub -> Project -> Settings -> Secretes -> Create Secretes

Cant create GITHUB_TOKEN give name for this as CR_TOKEN

<br/>

    CR_TOKEN

<br/>

There were errors with new configs with GPG. I tried to work without any:

    GPG_KEYRING_BASE64
    GPG_PASSPHRASE

<br/>

## How to run apps

I am working in ubuntu 20.04.1 LTS

Docker, Minikube, Kubectl, Skaffold should be installed.

<br/>

### Docker

```
$ docker -v
Docker version 20.10.0, build 7287ab3
```

<br/>

### Minikube installation

```
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

```

<br/>

```
$ minikube version
minikube version: v1.16.0
```

<br/>

### Kubectl installation

```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

$ kubectl version --client --short
Client Version: v1.20.2

```

<br/>

### Run minikube

```
$ {
    minikube --profile my-profile config set memory 8192
    minikube --profile my-profile config set cpus 4

    // minikube --profile my-profile config set vm-driver virtualbox
    minikube --profile my-profile config set vm-driver docker

    minikube --profile my-profile config set kubernetes-version v1.20.2
    minikube start --profile my-profile
}
```

<br/>

    // Enable ingress
    $ minikube addons --profile my-profile enable ingress

<br/>

    $ minikube --profile my-profile ip
    172.17.0.2

<br/>

    $ sudo vi /etc/hosts

```
#---------------------------------------------------------------------
# Minikube
#---------------------------------------------------------------------
172.17.0.2 cats.app
```

<br/>

### Helm installation

    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

    $ kubectl config view

    $ helm version --short
    v3.5.0+g32c2223

<br/>

### Working with Charts Repo

```
$ curl -v http://k8s-helm-repo.jsdev.org/index.yaml


***
apiVersion: v1
entries:
  cats-app:
  - apiVersion: v1
    appVersion: 0.1.1
    created: "2021-01-16T12:55:03.286810338Z"
    description: Cats Appication 0.1.1
    digest: 1534c8995b653c31ff6946cb751d4c226762c5b85bde7237d23c03b8d3cabff8
    name: cats-app
    urls:
    - https://github.com/webmakaka/Creating-a-Helm-Chart-Repository-using-Github-Actions/releases/download/cats-app-0.1.1/cats-app-0.1.1.tgz
    version: 0.1.1
generated: "2021-01-16T12:55:03.134263726Z"
* Connection #0 to host k8s-helm-repo.jsdev.org left intact
```

<br/>

    $ helm repo add jsdev http://k8s-helm-repo.jsdev.org

<br/>

    $ helm repo update

<br/>

    $ helm search repo jsdev/
    NAME          	CHART VERSION	APP VERSION	DESCRIPTION
    jsdev/cats-app	0.1.1        	0.1.1      	Cats Appication 0.1.1

<br/>

    $ helm install cats-app jsdev/cats-app

<br/>

```
$ helm list
NAME    	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART         	APP VERSION
cats-app	default  	1       	2021-01-16 16:01:58.161204382 +0300 MSK	deployed	cats-app-0.1.1	0.1.1
```

<br/>

```
$ helm status cats-app
NAME: cats-app
LAST DEPLOYED: Sat Jan 16 16:01:58 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

<br/>

```
$ kubectl get pods
NAME                                            READY   STATUS    RESTARTS   AGE
minikube-cats-app-deployment-66bd6cf4b5-5lr2s   1/1     Running   0          2m11s
minikube-cats-app-deployment-66bd6cf4b5-jwbnd   1/1     Running   0          2m11s
minikube-cats-app-deployment-66bd6cf4b5-q92lq   1/1     Running   0          2m11s
```

<br/>

```
http://cats.app/
OK
```

<br/>

### Remove everything

    $ helm delete cats-app
    $ helm repo remove jsdev


<br/>

**Also can be helpful for this theme**  
https://www.civo.com/learn/guide-to-helm-3-with-an-express-js-microservice

---

<br/>

**Marley**

Any questions in english: <a href="https://jsdev.org/chat/">Telegram Chat</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Телеграм чат</a>


