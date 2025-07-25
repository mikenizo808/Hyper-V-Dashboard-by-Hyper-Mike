# Hyper-V-Dashboard-by-Hyper-Mike
An example Grafana Dashboard for Hyper-V. This requires that the open source telegraf agent (by InfluxData) be installed on the machine to monitor. If you do not have `telegraf` yet, we guide you through installing it. Also, official vendor links for each step will be provided as well, such as for installing `Grafana` and `InfluxDB`.

## Example dashboard

[Example dashboard screenshot](https://github.com/mikenizo808/Hyper-V-Dashboard-by-Hyper-Mike/blob/main/Example%20dashboard%20screenshot.jpeg)


## Overview

This stats kit will use `Grafana` to display performance and other metrics for hyper-v.
The data can be collected using `telegraf` by `InfluxData` and we can write to any
datasource such as `InfluxDB`. The hyper-v dashboard works on all versions v1 to
v3 of `InfluxDB`.  All components are open source.

## Changelog

- 30March2025 - initial release
- 31March2025 - Made the dashboard slightly wider to fit panels better.
- 31March2025 - Renamed the panel "Virtual Machines" to "running VMs"
- 01April2025 - Changed some metrics from "latest" to "mean" so multi-day charts are accurate.
- 01April2025 - Added informational text to more panel descriptions. These describe the purpose of the panel when hovering over the `i` in the title bar.

> Note: Adding panel descriptions seems to give us less room for the actual title of each panel. If your screen is too small, you can edit and delete the descriptions, or press the appropriate button on your browser to resize your screen such as `CTRL+` or `CTRL-`. This kit works best on a big monitor, but remember you can always customize and put something important on its own line.

## License

MIT

## Credit

The hyper-mike dashboard for hyper-v is inspired by but not based on the hyper-v dashboard written in 2018, by user [allangood](https://grafana.com/orgs/allangood).

[https://grafana.com/grafana/dashboards/2618-hyper-v-metrics/](https://grafana.com/grafana/dashboards/2618-hyper-v-metrics/)

We attempted to preserve the stats collection naming to include all of the above and more.

The true prior art we borrow is the demonstration dashboard from `grafana labs`, dashboard ID `928`.
That dashboard was edited to include the desired items and then exported for use in another instance.
We then shared that `json` copy of the dashboard here.

## Plan

First we install `grafana`, then `InfluxDB` and finally `telegraf`, though you could do them in any order.
Here we install `grafana` and `influxdb` on a dedicated Linux machine though that is optional. 

> Note: If you already have `grafana` and `influxdb` (or similar) setup, you can skip to the `telegraf` section.

## Requirements

This setup would run fine on smaller instances, we are using 4 GB
of RAM and 4 CPU for the stats collector machine which we happen
to run on Linux, but all software packages in this stack are
cross-platform.

## minimalist configs

Optionally, see my related repo to review some minimalist configurations.
These contain the least information required to run the various packages such as
`InfluxDB`, `Grafana` and `Telegraf`.

[https://github.com/mikenizo808/minimalist-oss-configurations](https://github.com/mikenizo808/minimalist-oss-configurations)


## Install `grafana`

You can install grafana on many plaforms. This is the main page to get started.

    https://grafana.com/docs/grafana/latest/setup-grafana/installation/


## Example setup steps for `grafana` on `Ubuntu 24.04`


    ## basic installation steps
    https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/

    ## create a self-signed cert (not for production)
    https://grafana.com/docs/grafana/latest/setup-grafana/set-up-https/

    ## Edit your grafana.ini
    sudo nano /etc/grafana/grafana.ini

    ## Or exclusive of the above, copy a file instead of editing
    ##
    ## For a simple grafana config, you can use my related repo https://github.com/mikenizo808/minimalist-oss-configurations
    sudo cp minimalist-grafana.ini /etc/grafana/grafana.ini

    ## Start the grafana service
    https://grafana.com/docs/grafana/latest/setup-grafana/start-restart-grafana/

    ## Open a browser to some local or remote address
    https://127.0.0.1:3000

    ## Review the user interface
    ##
    ## You will not have any data yet. In a later step, we add an InfluxDB "Connection" for your default datasource.


## Install `InfluxDB 3 Core (beta)`

For `Ubuntu 24.04` the only requirement before installing is `curl`, so simply `sudo apt install curl` and then follow the vendor instructions from the `InfluxData` team.

[https://docs.influxdata.com/influxdb3/core/install/](https://docs.influxdata.com/influxdb3/core/install/)

> Note: There is a 3 day retention limit on stats when using the default `InfluxDB v3 Core` product, so some prefer `InfluxDB v2` for long term stats. Also notable is that the full Enterprise version of `InfluxDB v3 Core` is available for home use upon request from the `InfluxData` team.

## Download `telegraf`

Navigate to [https://influxdata.com/downloads](https://influxdata.com/downloads).

## Install `telegraf`

Follow the vendor-provided instructions to install on your desired platform.

## backup your default telegraf configuration

Here we assume you have PowerShell available. Substitute as needed.

    #for windows
    mkdir ~/backups
    Copy-Item "C:\Program Files\Telegraf\telegraf.conf" ~/backups/backup-of-default-telegraf.conf

    #for Linux (i.e. if monitoring your Linux node with telegraf)
    mkdir ~/backups
    sudo cp /etc/telegraf/telegraf.conf ~/backups/backup-of-default-telegraf.conf

## Telegraf custom configurations

By default `telegraf` can operate with both `inputs` and `outputs` in the same file.

On Linux we usually leave it all in one file `telegraf.conf` but on Windows, it is nice
to split them up when you can. Then you can lockdown the `outputs.conf` file, if desired.

Here, for this Windows example for a Hyper-V host, we split them into different files.
So `telegraf.conf` becomes `inputs.conf` and `outputs.conf`. Further, we create a
folder called `conf`.

> Note: This assumes you have `telegraf` installed already in the default location. Adjust path as needed.

    #create the conf folder (optional)
    mkdir "C:\Program Files\Telegraf\conf"

    #do the copy action
    Copy-Item "path-to-downloaded-hyper-mike-inputs.conf" "C:\Program Files\Telegraf\conf\inputs.conf"
    Copy-Item "path-to-downloaded-hyper-mike-outputs.conf" "C:\Program Files\Telegraf\conf\outputs.conf"

## Start or restart services to test

After making changes to configurations for `grafana` or `telegraf`, be sure to start or restart the service as needed.

> Tip: You can check your success using `telegraf -test` (that is one dash).

## Add a default datasource for `grafana`

Login to grafana using a browser and click on `Connections` in the left panel.
Then select `InfluxDB` for the datasource. Enter the relevant information and
then click test datasource.

> Note: If using `InfluxDB 3 Core (beta)` you will need to populate the `Custom http headers` section when adding your datasource. See the guide included with this repo called `How to add an InfluxDB 3 datasource in Grafana.md`. Spoiler, for the `Headers` text field, enter the word `Authorization`. For the `Value` text field, enter the word `Token` followed by some secret, such as `Token beta`.

> Tip: For `InfluxDB 3 Core (beta)` the `Token` is not validated.

## Review your datasources

After adding a datasource, you can go back and review it if desired.
However, you must expand the `Connections` drop-down to reveal the
sub-menu option called `Data sources`.

> Note: You can also click `Administration` and then click the provided link for `Connections`.

## Troubleshooting datasources

If you did not create a database in `InfluxDB`, then do that now and try again.
For example, you can use the `mydb` you created if following the official instructions
for `InfluxDB 3 Core (beta)`. If you want a full walk-through with more examples,
see my guide `Hyper Mike on InfluxDB 3 Core on Ubuntu 24.04.md` located in the repo
[https://github.com/mikenizo808/minimalist-oss-configurations](https://github.com/mikenizo808/minimalist-oss-configurations)

Note that `InfluxDB` should be running when you test your access from `Grafana`.

## Grafana default dashboard

If you have `Telegraf` writing stats to `InfluxDB`, and your default datasource setup in `Grafana`,
then we can proceed to adding and creating "dashboards" now.

Login to your `grafana` instance (i.e. `https://someaddress:3000`).
Then, navigate to `Dashboards > New > Import`.
Enter in the dashboard ID `928` to play with the very elegant official dashboard from the `grafana labs` team.

Have a look around, and understand that the default dashboard has a lot of metrics that are `Linux` only.
So `Windows` servers may show errors when viewing those metrics.

## The hyper mike dashboards

My dashboards are just a customized version of the above. Then I added support for all of the `Windows` metrics that do not show on the default dashboards, and in this case some metrics about hyper-v.

The hyper mike dashboard is windows-aware. This assumes you are collecting the proper stats with `telegraf` or similar.

