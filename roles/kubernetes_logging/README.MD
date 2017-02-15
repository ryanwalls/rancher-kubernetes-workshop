# kubernetes_logging role
This role is used to setup logging to Sumologic from a Kubernetes cluster using fluentd.
See: https://github.com/SumoLogic/fluentd-kubernetes-sumologic

## Variables
* `kubernetes_logging_sumologic_source_url` - url of sumologic source
* `kubernetes_logging_daemonset_name` - name of daemonset (fluentd)
