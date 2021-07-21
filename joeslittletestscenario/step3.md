# Optional: explore kapp

You can skip this step if you want to get straight to kapp-controller

### Using kapp to install a cronjob

First clone the repo for examples

`git clone https://github.com/vmware-tanzu/carvel-kapp`{{execute}}

then deploy an example job!

`kapp deploy -a hellocron -f carvel-kapp/examples/jobs/cron-job.yml -y`{{execute}}

Now take a look!

`kapp ls`{{execute}}

`kapp inspect -a hellocron --tree`{{execute}}

We scheduled our cron job to output a hello message every minute, so if you're
patient you'll see new messages appended to the logs:

`kapp logs -f -a hellocron`{{execute}}

When you're done watching the logs you can use control-c (`^C`{{execute ctrl-seq}}) to quit.

Because this was an optional interlude, we can use kapp to uninstall the cron
job before proceeding:
`kapp delete -a hellocron -y`{{execute}}
