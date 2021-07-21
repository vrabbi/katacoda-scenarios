## Creating a package

We will be using ytt templates that describe a simple Kubernetes Deployment and Service.
These templates will install a simple greeter app with a templated hello message.

Create a config.yml:

```
cat > config.yml << EOF
#@ load("@ytt:data", "data")

#@ def labels():
simple-app: ""
#@ end

---
apiVersion: v1
kind: Service
metadata:
  namespace: default
  name: simple-app
spec:
  ports:
  - port: #@ data.values.svc_port
    targetPort: #@ data.values.app_port
  selector: #@ labels()
---
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: default
  name: simple-app
spec:
  selector:
    matchLabels: #@ labels()
  template:
    metadata:
      labels: #@ labels()
    spec:
      containers:
      - name: simple-app
        image: docker.io/dkalinin/k8s-simple-app@sha256:4c8b96d4fffdfae29258d94a22ae4ad1fe36139d47288b8960d9958d1e63a9d0
        env:
        - name: HELLO_MSG
          value: #@ data.values.hello_msg
EOF
```{{execute}}

and a values.yml:

```
cat > values.yml <<- EOF
#@data/values
---
svc_port: 80
app_port: 80
hello_msg: stranger
EOF
```{{execute}}

## Package Contents
The first step in creating our package is to create an [imgpkg bundle](https://carvel.dev/imgpkg/docs/latest/resources/#bundle)
that contains the package contents: the above configuration (config.yml and values.yml) and a reference to the greeter app image (docker.io/dkalinin/k8s-simple-app@sha256:...).

To start, lets create a directory with the above configuration files:
```
mkdir -p package-contents/config/
mv config.yml package-contents/config/config.yml
mv values.yml package-contents/config/values.yml
```{{execute}}

([Package bundle format](https://carvel.dev/kapp-controller/docs/latest/packaging/#package-bundle-format) describes the purpose of each directory as well as general recommendations.)

Once we have the configuration figured out, let’s use kbld to record which container images are used:
```
mkdir -p package-contents/.imgpkg
kbld -f package-contents/config/ --imgpkg-lock-output package-contents/.imgpkg/images.yml
```{{execute}}

For more on using kbld to populate the .imgpkg directory with an ImagesLock, and why it is useful, see the [imgpkg docs on the subject]("imgpkg docs on the subject"
https://carvel.dev/kapp-controller/docs/latest/package-authoring/#:~:text=imgpkg%20docs%20on%20the%20subject).

Once these files have been added, our package contents bundle is ready to be pushed!

For the purpose of this tutorial, we will run an unsecured local docker
registry. In the real world please be safe and use appropriate security
measures.

`docker run -d -p 5000:5000 --restart=always --name registry registry:2`{{execute}}

From the terminal we can access this registry as `localhost:5000` but within the
cluster we will need to grab the IP Address. To emphasize that you would
normally use a repo host such as dockerhub or harbor we will store the IP
address in a variable:

```
export REPO_HOST="`ifconfig | grep -A1 docker | grep inet | cut -f10 -d' '`"
```{{execute}}

Now we can publish our bundle to our registry:

`imgpkg push -b ${REPO_HOST}:5000/packages/simple-app:1.0.0 -f package-contents/`{{execute}}
