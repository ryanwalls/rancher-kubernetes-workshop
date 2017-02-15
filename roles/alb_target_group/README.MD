# alb_target_group role
This role is used to create an ALB target group.  More info here: http://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html

## Variables
* `alb_target_group_name` - name of the target group
* `alb_target_group_protocol` - protocol for the target group e.g. https or http
* `alb_target_group_port` - port for the target group e.g. 80
* `alb_target_group_vpc_id` - vpc id for the target group (default is the default vpc)
* `alb_target_group_health_check_protocol` - protocol to use for the health check e.g. https or http
* `alb_target_group_health_check_port` - port to use for the health check e.g. 80
* `alb_target_group_health_check_path` - path to use for the health check e.g. /status
* `alb_target_group_health_check_interval_seconds` - the approximate amount of time, in seconds, between health checks of an individual target. Allowed range: 5-300. (30 seconds)
* `alb_target_group_health_check_timeout_seconds` - the amount of time, in seconds, during which no response from a target means a failed health check, must be less than health check interval. Allowed range: 2-inf. (5 seconds)
* `alb_target_group_healthy_threshold_count` - the number of consecutive health checks successes required before considering an unhealthy target healthy. The default is 5
* `alb_target_group_unhealthy_threshold_count` - the number of consecutive health check failures required before considering a target unhealthy. The default is 2.
* `alb_target_group_healthy_response_code` - the HTTP codes. The default value is 200. You can specify multiple values (for example, "200,202") or a range of values (for example, "200-299").








