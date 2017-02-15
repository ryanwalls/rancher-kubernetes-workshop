# kubernetes_quayio role
This role is used to give Kubernetes access to the 3dsim Quay.io repository.
For this to work, make sure the `3dsim+rancher` user in quay.io has access to all the repositories.

## Variables
* `kubernetes_quayio_secret_name` - name of quay.io pull secret (rancher-pull-secret)
