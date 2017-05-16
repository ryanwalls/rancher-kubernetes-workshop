# kubernetes_cluster_autoscaler role
This role is used to deploy a kubernetes cluster autoscalar for aws.
See:
- https://github.com/kubernetes/contrib/tree/master/cluster-autoscaler
- https://github.com/kubernetes/charts/tree/master/stable/aws-cluster-autoscaler
- https://github.com/kubernetes/kops/tree/master/addons/cluster-autoscaler

Would like to be able to set the mechanics and thermal clusters to minimum size 0, but currently the minimum size has to be 1.
Follow this issue: https://github.com/kubernetes/contrib/issues/2425

## Variables
* `kubernetes_cluster_autoscaler_name` - name of deployment to create, will also be used for labeling purposes (cluster-autoscalar)
* `kubernetes_cluster_autoscaler_image` - image to deploy (gcr.io/google_containers/cluster-autoscaler)
* `kubernetes_cluster_autoscaler_image_tag` - tag for the image (latest)
* `kubernetes_cluster_autoscaler_core_asg` - name of the core ASG used for hosting k8s system containers ({{ env }}-kubernetes-cluster-asg)
* `kubernetes_cluster_autoscaler_core_asg_min` - minimum number of core ASG nodes (2)
* `kubernetes_cluster_autoscaler_core_asg_max` - maximum number of core ASG nodes (25)

