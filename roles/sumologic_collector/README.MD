# sumologic_collector role
This role is used to setup a Sumologic collector

## Variables
* `sumologic_collector_name` - name of sumologic collector
* `sumologic_collector_description` - description for sumologic collector
* `sumologic_collector_sumologic_api_url` - url of sumologic api (https://api.us2.sumologic.com/api/v1)

## Outputs
* `sumologic_collector_url_fact` - url of sumologic collector e.g. https://api.us2.sumologic.com/api/v1/collectors/113059250/sources
* `sumologic_collector_id_fact` - id of sumologic collector
