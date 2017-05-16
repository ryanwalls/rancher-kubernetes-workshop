# rancher-kubernetes-workshop
Supporting code/diagrams for my devopsdays SLC workshop on running Kubernetes with Rancher given on May 16, 2017.

This repo has all the code necessary to create a k8s cluster in a Rancher environment with logging to Sumologic, DNS through AWS route53,
and alerting through VictorOps.  This is configured to match what we do at 3dsim.  You will probably want to modify many things to match your needs.

In this workshop you will learn how to deploy a production grade, highly available Kubernetes cluster in AWS using Rancher and Ansible.  Almost all steps will be automated so that the finished product truly embodies Infrastructure as Code.  By the end of the workshop, we will have a fully running kubernetes cluster, authentication/authorization in place for cluster management, monitoring and alerting using Prometheus, graphs of server performance using Grafana, rolling deploys of application code, centralized logging, and more.

Read more about the various tools/platforms:
https://kubernetes.io
http://rancher.com
https://www.ansible.com

Requirements: You will need to have an AWS account set up and able to provision new servers.  You will also need to have git installed to download the workshop material.  A recent version of Docker is also required (https://www.docker.com).  If you don't have an AWS account, you can sign up for one here: https://portal.aws.amazon.com/gp/aws/developer/registration/index.html

## Prerequisites
* Docker installed.  https://www.docker.com
* AWS account  https://portal.aws.amazon.com/gp/aws/developer/registration/index.html
* Environment variables set for AWS access.  `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.  See
http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html for how to generate a key and secret.  You will probably need to create a user.  If you do, give them admin or power user privileges.
* SSL cert for a domain that you control.  See https://aws.amazon.com/blogs/aws/new-aws-certificate-manager-deploy-ssltls-based-apps-on-aws
If you need to register a domain, .me.uk is cheapest on route53 for $8.  Set it up in us-east (N Virginia) region.
* Install Kubernetes `kubectl` command line tool, v1.5.4.  See https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-via-curl.  On Mac you'll run something like this:
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.5.4/bin/darwin/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

## Overview of workshop
* Create Rancher server
* Setup auth for rancher
* Create Kubernetes cluster
* Deploy applications to cluster
* Update applications in cluster
* Setup prometheus, grafana, and alertmanager for monitoring/alerting
* Demo centralized logging
* (Extra credit) Setup monitoring of application using prometheus
* (Extra credit 2) Setup centralized logging using ELK stack

## Create Rancher server (Optional)
* Run the following command in the root of this repo to create a container called `ansible`
```
docker run --restart=always -d -v `echo ~`/.ssh:/root/.ssh -e AWS_ACCESS_KEY_ID=`echo $AWS_ACCESS_KEY_ID` -e AWS_SECRET_ACCESS_KEY=`echo $AWS_SECRET_ACCESS_KEY` --name ansible ryanwalls/ansible-aws:v2.2.1.0-1-k8s tail -f /dev/null
```
* (Optional) Change password in `rancher-server.yml` lines 67 and 92
* Create an ssh key pair in AWS and fill in `keyName` and `ansible_ssh_private_key_file` variables in `group_vars/all/main.yml` with name and location of an ssh key you've created in AWS and downloaded to your machine.
Use a relative path.  Download your key to ~/.ssh on your desktop.  See http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair.  Ideally this would be automated.
* If you haven't already, you'll need to setup an SSL cert.  See instructions in "Prerequisites" section.
* Fill in `default_ssl_cert_arn` in `group_vars/all/main.yml` with the ARN of your SSL cert
* Fill in `default_hosted_zone` in `group_vars/all/main.yml` with the domain of your route53 hosted zone (e.g. ryanwalls.com)
* Let's create the rancher server now.  Run the following from the root of this project.
```
docker cp . ansible:ansible && docker exec -it ansible ansible-playbook -i inventory/ -vvvv --extra-vars "env=slc" rancher-server.yml
```

## Setup auth for rancher (optional)
* From Rancher click... Admin -> Access Control
* Follow instructions to setup Github authentication

## Create Kubernetes cluster
* Fill in `rancher_api` variables in `group_vars/all/main.yml`.
* Fill in `default_ssl_cert_arn` in `group_vars/all/main.yml` with the ARN of your SSL cert if you didn't do it earlier.
* Fill in `default_vpc_id` in `group_vars/all/main.yml` with the VPC id of your default vpc.  The default VPC can be found in your ec2 dashboard on the right hand side.
* Fill in `default_hosted_zone` in `group_vars/all/main.yml` with the domain of your route53 hosted zone
* Create environment in Rancher with Kubernetes as the orchestration tool
* Create an environment specific API key (API -> Keys -> Advanced Options -> Add Environment API Key)
* Copy key into `group_vars/all` with the matching environment.
* Also update the environment id in `group_vars/all`.  Id is in the URL for rancher e.g. `https://rancher.3dsim.com/env/1a644/api/keys`, so `1a644` is the id.
* Run `kubernetes-cluster.yml` with env variable set from the root of this repo.  e.g.

```
docker cp . ansible:ansible && docker exec -it ansible ansible-playbook -i inventory/ -vvvv --extra-vars "env=slc" kubernetes-cluster.yml
```

* Once kubernetes environment is up and running, generate a key for kubernetes.  In Rancher navigate to Kubernetes -> CLI -> Generate Config.
Put username and password for kubernetes in `group_vars/all`
* Setup logging by running `kubernetes-logging.yml`
* Setup monitoring


## Deploy applications to cluster
* Setup default service route (used for healthchecks) `kubernetes-default-service.yml`
* To deploy an application look at `kubernetes-example1-deploy.yml`.  Make sure and add the ingress vars in
`roles/kubernetes_ingress/vars/main.yml` to match the application you're deploying.

## Update applications in cluster

## Setup prometheus, grafana, and alertmanager for monitoring/alerting
* Clone https://github.com/3DSIM/kube-prometheus.
* Make sure you have setup your kube config file for the right environment.  In rancher, Kubernetes -> CLI -> Generate config.  Put config in ~/.kube/config.
  * If you have multiple contexts (See sample kubeconfig below), you can switch between them by running `kubectl config set current-context <context name>`
* Execute `./hack/cluster-monitoring/deploy`
* Go back to infrastructure project and setup victorops routing keys in `roles/kubernetes_prometheus_config/vars/main.yml`
* Run `kubernetes-monitoring.yml`

## Demo centralized logging




* Fill in `sumologic_access_id` and `sumologic_access_key` variables in `group_vars/all/main.yml`
* Fill in `kubernetes_prometheus_config_victorops_key` in `roles/kubernetes_prometheus_config/vars/main.yml` with the key for your victorops account.


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