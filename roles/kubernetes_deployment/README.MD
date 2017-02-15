# kubernetes_deployment role
This role is used to deploy kubernetes Deployment objects

## Variables
* `kubernetes_deployment_app_name` - name of app, will be used for labeling purposes
* `kubernetes_deployment_name` - name of deployment to create (kubernetes_deployment_app_name)
* `kubernetes_deployment_replicas` - number of pods to run (2)
* `kubernetes_deployment_image` - image to deploy
* `kubernetes_deployment_image_tag` - tag for the image (latest)
* `kubernetes_deployment_container_port` - port for the container
