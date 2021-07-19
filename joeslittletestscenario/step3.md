## Hey I Thought This Was a Kubernetes Tutorial
oh yeah cool. 

`kubectl get all -A`{{execute}}

look how much is already running just from the image! We have a whole cluster.

## But with kapp too, right?

`kapp deploy -a app1 -f examples/simple-app-example/config-1.yml`{{execute}}

Now take another look!
`kapp ls`{{execute}}

`kapp inspect -a app1 --tree`{{execute}}

`kapp logs -f -a app1`{{execute}}

if you click this kubectl command it should interrupt the logs tail that we left
running.
`kubectl get all -A`{{execute interrupt}}
