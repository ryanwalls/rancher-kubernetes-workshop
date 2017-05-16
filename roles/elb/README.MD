# elb ansible role
Role to spin up an ELB and associated security group.

## Variables

### Input Variables
Variables used in this role are listed below.  Defaults are in parentheses.

* `region` - the region to spin up the instance in (us-west-2).
* `zones` - the availability zones to create instances in and for the ELB to distribute
requests to ([us-west-2a, us-west-2b])
* `env` - the environment the app infrastructure will be created in e.g. `dev`, `qa`, `prod` (dev)
* `elb_app_name` - the name of the app to spin up, this will be used to create names for security groups and instances.
* `elb_instance_port` - the port to open to the outside world
* `elb_zones` - AWS zones where servers will be created, ([us-west-2a, us-west-2b])

### Registered Output Variables
* 'elb' - object describing the ELB that was created
* 'elb_security_group' - object describing the security group associated with this ELB
