# kubernetes_dns role
This role is used to create a route53 DNS entry that points to the ALB for a kubernetes cluster.

## Variables
* `kubernetes_dns_target_alb_name` - name of ALB to create A record for
* `kubernetes_dns_record` - DNS record to create as an A record for the kubernetes ALB
