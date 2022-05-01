# InfluxDb

https://medium.com/geekculture/deploying-influxdb-2-0-using-docker-6334ced65b6c

First we create a docker compose file that runs influxdb (2.x) on port 8086.
We assume that theres an `influxdb/influxdb2` folder in the directory where this compose file is.
The volumes in the compose file then allow for influx to persist the data to our local machine.

Once you 
> docker-compose up -d

You can access influx by navigating to
> localhost:8086

Using the environment variable file it will setup a user, bucket, and hardcoded token so you dont have to go through the setup screen in the UI.


# Grafana

https://medium.com/swlh/easy-grafana-and-docker-compose-setup-d0f6f9fcec13

To connect to the influxdb service you need to setup a network and have both services use it, and then each service can be reached by the service name. In order to create the influxdb datasource you need to use a custom http header:

    Fixed it. Turns out I needed to include an HTTP Header, key “Authorization”, value “Token myToken”.

Because we're using influx 2.x, it uses buckets instead of databases and retention policies. So in order to use InfluxQl with influx 2.x you need create DBRP mappings for unmapped policies:

    influx v1 dbrp create \
        --org myorg \
        --token [token_goes_here] \
        --db sensor \
        --rp example-rp \
        --bucket-id [the_bucket_id] \
        --default

https://ivanahuckova.medium.com/setting-up-influxdb-v2-flux-with-influxql-in-grafana-926599a19eeb
https://docs.influxdata.com/influxdb/v2.0/query-data/influxql/#map-unmapped-buckets

For provisioning the datasource automatically:

https://grafana.com/docs/grafana/latest/datasources/influxdb/provision-influxdb/#influxdb-2x-for-influxql-example
https://grafana.com/docs/grafana/latest/administration/provisioning/#datasources


# E*Trade

You need to login to your account and create a Sandbox API Key/Secret from this link:
> https://us.etrade.com/etx/ris/apikey

