# Installing Kapp
### Code Preformatting
Here's the script for installing kapp onto katacoda

<pre><code class="language-bash">
#!/bin/bash

if test -z "$BASH_VERSION"; then
  echo "Please run this script using bash, not sh or any other shell." >&2
  exit 1
fi

install() {
	set -euo pipefail

	# Start Kubernetes on Katacoda
	launch.sh

	wget -O- https://carvel.dev/install.sh | bash

	git clone https://github.com/vmware-tanzu/carvel-kapp
	echo "Cloned github.com/vmware-tanzu/carvel-kapp for examples"
}

install
</code>
</pre>

### Now install kapp
and here's the curl command to install it right now!

`wget -O- https://carvel.dev/kapp-install-katacoda.sh | bash`{{execute}}

