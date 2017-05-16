#rancher_cluster(an ansible role)

## Goal
Create and deploy an autoscaling ECS cluster

## Variables
Defaults are in parentheses
* `rancher_cluster_ami` - the ami to use for the cluster servers (ami-9abea4fb)
* `rancher_cluster_key_name` - name of the AWS access key pair to use when creating the cluster (webapp-key-pair-uswest-oregon)
* `rancher_cluster_region` - AWS region in which to create the cluster (us-west-2)
* `rancher_cluster_name` - the name of the cluster to create, the autoscaling group
and the launch configuration will be named based on this name
* `rancher_cluster_security_group_name` - name of security group for the created cluster
* `rancher_cluster_instance_profile_name` - the name of the iam profile to use for the servers in the cluster (swf-{{env}})
* `rancher_cluster_instance_type` - the instance type to use in the cluster
* `rancher_cluster_min_size` - the minimum number of servers in the cluster (0)
* `rancher_cluster_max_size` - the maximum number of servers in the cluster (25)
* `rancher_cluster_asg_default_cooldown` - the default cooldown for the autoscaling group, in seconds (660)
* `rancher_cluster_instance_type` - the instance type for the cluster
* `rancher_cluster_launch_configuration_version` - version number for the launch configuration (LC). __This must be updated for any LC changes to take affect. The caller must provide this parameter.__ See solver-cluster.yml for an example.
* `rancher_cluster_registration_url` - url for registering new hosts on the cluster
* `rancher_cluster_is_data_plane` - if true, sets up etcd backups to s3 and makes sure etcd is the only thing running (false)
* `rancher_cluster_is_orchestration_plane` - if true, only allows orchestration tools, like kubernetes to run (false)
* `rancher_cluster_is_compute_plane` - if true, only allows compute resources to run (false)


