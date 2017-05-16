# elb_dns role
This role is used to create a route53 DNS entry that points to an ELB.

## Variables
* `elb_dns_target_elb_name` - name of ELB to create A record for
* `elb_dns_record` - DNS record to create as an A record for the ELB
