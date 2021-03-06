# alb ansible role
Role to spin up an ALB and associated security group.

## Variables

### Input Variables
Variables used in this role are listed below.  Defaults are in parentheses.

* `region` - the region to spin up the instance in (us-west-2).
* `zones` - the availability zones to create instances in and for the ALB to distribute
requests to ([us-west-2a, us-west-2b])
* `env` - the environment the app infrastructure will be created in e.g. `dev`, `qa`, `prod` (dev)
* `tear_down` - if true, will tear down everything this role creates (false)
* `alb_app_name` - the name of the app to spin up, this will be used to create names for security groups and instances.
* `alb_instance_port` - the port to open to the outside world (9000)
* `alb_zones` - AWS zones where servers will be created, ([us-west-2a, us-west-2b])
* `alb_default_action_target_group` - target group to forward requests to by default

### Registered Output Variables
* 'alb' - object describing the ALB that was created
* 'alb_security_group' - object describing the security group associated with this ALB
