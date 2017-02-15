#  security_group ansible role
Role to create a security group

## Variables
Variables used in this role are listed below.  Defaults are in parentheses.

* `security_group_outside_world_ports` - the list of ports to open to the outside world e.g. [8200, 8300, 8400]
* `security_group_intra_group_all_ports` - when true, opens all ports within the security group (false)
* `security_group_intra_group_tcp_ports` - the list of tcp ports to open to other instances in the same security_group e.g. [8200, 8300, 8400]
* `security_group_intra_group_udp_ports` - the list of udp ports to open to other instances in the same security_group e.g. [8200, 8300, 8400]
* `security_group_custom_port_rules` - the list of custom port rules to append to the existing security
group rules.  These should be in the same format as regular security group rules.  e.g.
```
security_group_custom_port_rules:
   - proto: tcp
     from_port: 8500
     to_port: 8500
     cidr_ip: "{{ vpn_server_ip }}/32"
```
* `security_group_application_name` - the name of the app that will be associated with
this security group, used to name the sg.
* `region` - the region to spin up the instance in (us-west-2).
* `useEnv` - if true, names the security groups based on the environment.
* `env` - the environment e.g. `dev`, `qa`, `prod` (dev)
* `tear_down` - if true, will tear down everything this role creates (false)
