# Optional: explore kapp

You can skip this step if you want to get straight to kapp-controller

### kapp example app

First clone the repo for examples

`git clone https://github.com/vmware-tanzu/carvel-kapp`{{execute}}

then deploy an example app!

`kapp deploy -a app1 -f carvel-kapp/examples/simple-app-example/config-1.yml`{{execute}}

Now take a look!

`kapp ls`{{execute}}

`kapp inspect -a app1 --tree`{{execute}}

`kapp logs -f -a app1`{{execute}}

### Interrupting a running process

if you click this kubectl command it should interrupt the logs tail that we left
running.

`kubectl get all -A`{{execute interrupt}}
