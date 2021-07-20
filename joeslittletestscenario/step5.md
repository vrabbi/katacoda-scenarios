# I believe I was promised kapp-controller?

Use kapp to install kapp-controller:

`yes | kapp deploy -a kc -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/download/v0.21.0/release.yml`{{execute}}

gaze upon the splendor!

`kubectl get all -n kapp-controller`{{execute}}


