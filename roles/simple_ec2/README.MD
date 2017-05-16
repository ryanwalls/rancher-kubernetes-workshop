# simple_ec2 ansible role

Role to spin up a simple EC2 instance with a security group setup.  After the play
finishes a host group will be created with the name `ec2hosts-{{ appName }}`.  The
group will contain the EC2 instance whether it was created in this play or not.


## Variables

Variables used in this role are listed below.  Defaults are in parentheses.

* `volumeSize` - set the size of the volume to create (30)
* `volumeDeviceName` - the volume device name (/dev/xvda)
* `simple_ec2_outside_world_ports` - the list of ports to open to the outside world e.g. [8200, 8300, 8400]
* `simple_ec2_custom_port_rules` - the list of custom port rules to append to the existing security
group rules.  These should be in the same format as regular security group rules.  e.g.
```
simple_ec2_custom_port_rules:
   - proto: tcp
     from_port: 8500
     to_port: 8500
     cidr_ip: "{{ vpn_server_ip }}/32"
```
* `ansibleConfigurationPort` - the port that ansible communicates over (22)
* `appName` - the name of the app to spin up, this will be used to create
names for security groups and instances.
* `region` - the region to spin up the instance in (us-west-2).
* `useEnv` - if true, spins up an instance that is tied to the `env` variable.
 e.g. `dev-myinstance-etc`.  If false, only ever spins up one instance in the given region.
* `env` - the environment e.g. `dev`, `qa`, `prod` (dev)
* `tear_down` - if true, will tear down everything this role creates (false)
* `simple_ec2_instance_profile_name` - if set, will set the instance profile name to apply to the ec2 instance
* `simple_ec2_server_count` - number of servers to spin up (1)
* `instance_type` - if set, will create the specified instance type (t2.micro)
* `use_default_ubuntu_ami` - if true, uses an Ubuntu AMI specified in `defaults/main.yml` (false).
