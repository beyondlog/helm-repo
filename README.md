# Instruction

Copy **cert** from 'deploy agent' tab on Weclome page. Please visit https://login.tslog.tristonetech.com to login

* add Beyond Log helm chart repo

```sh
❯❯❯ helm repo add beyondlog 'https://raw.githubusercontent.com/beyondlog/helm-repo/master/'
❯❯❯ helm repo update
```

* check repo is added and Beyond Log charts are avaiable
```sh
❯❯❯ helm search beyondlog
NAME                  	CHART VERSION	APP VERSION	DESCRIPTION
beyondlog/tslog-heroku	0.1.0        	1.0        	Beyond Log Heroku http log drain agent helm package
beyondlog/tslog-kube  	0.1.0        	1.0        	Beyond Log tslog Kubernetes agent
```

* deploy Heroku HTTP log drain agent

```sh
❯❯❯ helm install --name heroku-web --namespace <myns> --set config.agtid=<id>,config.cert=<cert>,config.server=wss://injector.<domain>/mqtt,config.labels.env=<dev>,config.labels.app=heroku-web,ingress.domain=<mydomain>,heroku.user=user,heroku.passwd=secret beyondlog/tslog-heroku

# add log drain
❯❯❯ heroku drains:add https://user:secret@heroku.<myns>.<mydomain>/logs
```

* deploy Beyond Log Kubernetes agent. (Only supports json-file log-driver)

```sh
# deploy kube agent will send logs to injector.<myhost>.tslog.tristonetech.com
❯❯❯ helm install --name tslog-kube --namespace <myns> --set config.cert=<cert>,config.server=wss://injector.<myhost>.tslog.tristonetech.com/mqtt,config.labels.env=<myenv> beyondlog/tslog-kube
```