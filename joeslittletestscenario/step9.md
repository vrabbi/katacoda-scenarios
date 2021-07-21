##Adding a PackageRepository

kapp-controller needs to know which packages are available to install.
One way to let it know about available packages is by creating a package repository.
To do this, we need a PackageRepository CR:

```
cat > repo.yml << EOF
---
apiVersion: packaging.carvel.dev/v1alpha1
kind: PackageRepository
metadata:
  name: simple-package-repository
spec:
  fetch:
    imgpkgBundle:
      image: localhost:5000/packages/my-pkg-repo:1.0.0
EOF
sed "s/localhost/`ifconfig | grep -A1 docker | grep inet | cut -f10 -d' '`/" -i repo.yml
```{{execute}}

(Because we're leveraging our localhost docker repo we're doing a little juggling
to get the IP address of the repo into the yaml so that imgpkg can find it
running within the cluster, see our
[demo video](https://www.youtube.com/watch?v=PmwkicgEKQE) and [website](https://carvel.dev/kapp-controller/docs/latest/package-consumption/#adding-package-repository) examples for more typical
use-case against an external repository.)

This PackageRepository CR will allow kapp-controller to install any of the
packages found within the `localhost:5000/packages/my-pkg-repo:1.0.0` imgpkg bundle, which we
stored in our docker OCI registry previously.

We can use kapp to apply it to the cluster:
`kapp deploy -a repo -f repo.yml -y`{{execute}}

Check for the success of reconciliation to see the repository become available:
`kubectl get packagerepository`{{execute}}

Once the deploy has finished, we are able to list the package metadatas to see, at a high level, which packages are now available within our namespace:
`kubectl get packagemetadatas`{{execute}}

If there are numerous available packages, each with many versions, this list can become a bit unwieldy, so we can also list the packages with a particular name using the --field-selector option on kubectl get.
`kubectl get packages --field-selector spec.refName=simple-app.corp.com`{{execute}}

From here, if we are interested, we can further inspect each version to discover
information such as release notes, installation steps, licenses, etc. For
example:
`kubectl get package simple-app.corp.com.1.0.0 -o yaml`{{execute}}


