# kubernetes_kubeconfig role
This role is used to create a kubeconfig.  More info here: https://kubernetes.io/docs/user-guide/kubeconfig-file

## Variables
* `kubernetes_kubeconfig_server` - kubernetes server url ("{{ rancher_api[env].host }}/{{ kubernetes_api[env].path }}")
* `kubernetes_kubeconfig_environment` - environment to use ("{{ env }}")
* `kubernetes_kubeconfig_username` - username to use ("{{ kubernetes_api[env].username }}")
* `kubernetes_kubeconfig_password` - password to use ("{{ kubernetes_api[env].password }}")
* `kubernetes_kubeconfig_insecure_skip_tls_verify` - skip tls verification (true)
* `kubernetes_kubeconfig_destination` - destination to put kubeconfig file (~/.kube/config)


