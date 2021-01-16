# [YouTube, Paul C] Creating a Helm Chart Repository using Github Actions [ENG, 2020]

**The idea is from YouTube Video:**  
https://www.youtube.com/watch?v=hL-8Jn5RTmw

<br/>

**Demo**  
https://github.com/helm/charts-repo-actions-demo

<br/>

## Prepare

    $ git checkout --orphan gh-pages
    $ rm -rf *
    $ rm -rf .gihtub
    $ rm -rf .gitignore
    $ git add --all
    $ git commit -m "empty gh-pages branch"
    $ touch index.html

<br/>

```
<h1>Helm Chart Repo</h1>
```

<br/>

    $ touch CNAME

<br/>

```
k8s-helm-repo.jsdev.org
```

<br/>

    $ git add --all
    $ git commit -m "index html"
    $ git push origin gh-pages

<br/>

### Domain Name Registrator

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

<!--

<br/>

### Working with Charts Repo

    $ curl -v https://charts-repo.webmakaka.com/index.yaml
    OK

<br/>

    $ helm repo add webmakaka https://charts-repo.webmakaka.com

    $ helm repo update

<br/>

    $ helm search repo webmakaka/
    NAME               	CHART VERSION	APP VERSION	DESCRIPTION
    webmakaka/guestbook	0.1.0        	1.0        	A Helm chart for Guestbook 1.0

<br/>

    $ helm install myguestbook webmakaka/guestbook

<br/>

    $ helm list
    NAME       	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART          	APP VERSION
    myguestbook	default  	1       	2020-04-13 05:43:26.200512363 +0300 MSK	deployed	guestbook-0.1.0	1.0

<br/>

    $ helm status myguestbook
    NAME: myguestbook
    LAST DEPLOYED: Mon Apr 13 05:43:26 2020
    NAMESPACE: default
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None

<br/>

http://frontend.minikube.local/

<br/>

### Remove everything

    $ helm delete myguestbook
    $ helm repo remove webmakaka

-->

---

<br/>

**Marley**

Any questions in english: <a href="https://jsdev.org/chat/">Telegram Chat</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Телеграм чат</a>
