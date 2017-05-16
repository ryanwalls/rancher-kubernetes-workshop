# kubernetes_iam_profile role
This role gives SWF, S3, SQS and ECS access to the kubernetes role.

Also this role sets up access needed by rancher hosts when setting up a k8s environment with AWS as cloud provider.  See https://github.com/rancher/rancher/issues/7829#issuecomment-280728877

## Variables
* `kubernetes_iam_profile_role_name` - Name of role to create
