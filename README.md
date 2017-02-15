# meetup-docker-in-production
Supporting code/diagrams for my Docker in Production talk given at the SLC Docker Meetup on Feb 15, 2017.

This repo has all the code necessary to create a k8s cluster in a Rancher environment with logging to Sumologic, DNS through AWS route53,
and alerting through VictorOps.  This is configured to match what we do at 3dsim.  You will probably want to modify many things to match your needs.

## Prerequisites
* Rancher server 1.4.0 running and accessible to outside world
* Environment variables set for AWS access.  `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.  See
http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html for how to generate a key and secret.
* Run the following command in the root of this repo to create a container called `ansible`
```
docker run -it -e AWS_ACCESS_KEY_ID=`echo $AWS_ACCESS_KEY_ID` -e AWS_SECRET_ACCESS_KEY=`echo $AWS_SECRET_ACCESS_KEY` --name ansible ryanwalls/ansible-aws:v2.2.1.0-1-k8s tail -f /dev/null
```
* Fill in `rancher_api` variables in `group_vars/all/main.yml`
* Fill in `keyName` variable in `group_vars/all/main.yml` with name of an ssh key you've created in AWS
* Fill in `sumologic_access_id` and `sumologic_access_key` variables in `group_vars/all/main.yml`
* Fill in `alb_certificate_arn` in `roles/alb/defaults/main.yml` with the ARN of your SSL cert
* Fill in `alb_target_group_vpc_id` in `roles/alb_target_group/defaults/main.yml` with the VPC id of your default vpc
* Fill in `kubernetes_ingress_domain` in `roles/kubernetes_ingress/defaults/main.yml` with the domain of your route53 hosted zone
* Fill in `kubernetes_prometheus_config_victorops_key` in `roles/kubernetes_prometheus_config/vars/main.yml` with the key for your victorops account.

## Setting up a Kubernetes cluster administered through Rancher
* Add 3dsim catalog, https://github.com/3DSIM/rancher-catalog.git, to rancher.  Admin -> Settings -> Add Catalog
* Create environment with 3dsim Kubernetes as the orchestration tool
* Create an environment specific API key (API -> Keys -> Advanced Options -> Add Environment API Key)
* Copy key into `group_vars/all` with the matching environment.
* Also update the environment id in `group_vars/all`.  Id is in the URL for rancher e.g. `https://rancher.3dsim.com/env/1a644/api/keys`, so `1a644` is the id.
* Run `kubernetes-cluster.yml` with env variable set from the root of this repo.  e.g.

```
docker cp . ansible:ansible && docker exec -it ansible ansible-playbook -i inventory/ -vvvv --extra-vars "env=qa" kubernetes-cluster.yml
```

* Once kubernetes environment is up and running, generate a key for kubernetes.  In Rancher navigate to Kubernetes -> CLI -> Generate Config.
Put username and password for kubernetes in `group_vars/all`
* Setup registry access by running `kubernetes-registries.yml`
* Setup logging by running `kubernetes-logging.yml`
* Setup default service route (used for healthchecks) `kubernetes-default-service.yml`
* Setup monitoring
  * Clone https://github.com/3DSIM/kube-prometheus.
  * Make sure you have setup your kube config file for the right environment.  In rancher, Kubernetes -> CLI -> Generate config.  Put config in ~/.kube/config.
    * If you have multiple contexts (See sample kubeconfig below), you can switch between them by running `kubectl config set current-context <context name>`
  * Execute `./hack/cluster-monitoring/deploy`
  * Go back to infrastructure project and setup victorops routing keys in `roles/kubernetes_prometheus_config/vars/main.yml`
  * Run `kubernetes-monitoring.yml`
* To deploy an application look at `kubernetes-example1-deploy.yml`.  Make sure and add the ingress vars in
`roles/kubernetes_ingress/vars/main.yml` to match the application you're deploying.


## Sample kubeconfig
Sample kubeconfig:

```
apiVersion: v1
kind: Config
clusters:
- cluster:
    api-version: v1
    server: "https://rancher.3dsim.com/r/projects/1a644/kubernetes"
  name: "qa"
- cluster:
    api-version: v1
    server: "https://rancher.3dsim.com/r/projects/1a923/kubernetes"
  name: "prod"
contexts:
- context:
    cluster: "qa"
    user: "qa"
  name: "qa"
- context:
    cluster: "prod"
    user: "prod"
  name: "prod"
current-context: "qa"
users:
- name: "qa"
  user:
    username: "<Your QA username>"
    password: "<Your QA secret>"
- name: "prod"
  user:
    username: "<Your Prod username>"
    password: "<Your Prod secret>"
```