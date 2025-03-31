# links


## get started with InfluxDB and Grafana

[https://grafana.com/docs/grafana/latest/getting-started/get-started-grafana-influxdb/](https://grafana.com/docs/grafana/latest/getting-started/get-started-grafana-influxdb/)


## For InfluxDB 3 Core (beta)

Use the intructions for adding an `InfluxDB 2` datasource when working with `InfluxDB 3 Core (beta)`.

[https://docs.influxdata.com/influxdb/v2/tools/grafana/](https://docs.influxdata.com/influxdb/v2/tools/grafana/)

> Note: In addition you will need to populate the `Custom http headers` section when adding a datasource in `grafana`.


## Populating the `Custom http headers` in `grafana`

This is needed if your datasource is `InfluxDB 3 Core (beta)`. When adding your `InfluxDB` datasource, you will need to populate
the `Custom http headers` section. It has two text fields you can type into, `Headers` and `Value`.


### `Headers`

For the `Headers` text field, enter the word `Authorization`.

    Authorization


### `Value`

For the `Value` text field, enter the word `Token` followed by some secret.

Here we enter some arbitrary text, such as the word `beta` to be the token. When you type this in the characters will be hidden. The syntax is exactly as follows. The word `Token` and then a space and finally, the secret text, in our case `beta`. Adjust as desired.

    Token beta

> Note: `InfluxDB 3 Core (beta)` does not need a valid token. This will change in a future release.