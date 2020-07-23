# crdhub

Helm Plugin

This plugin is used to satisfy api resource dependencies to allow out-of-order installation of helm charts that have dependencies on CRDs from other charts.

Use this tool to analyze a chart, determine the api components that are not yet defined, query the CRD Hub, retrieve CRDs from the Hub and install them prior to installing.

Maybe this plugin could be used to do the actual install, where it pulls any needed CRDs from the hub and does **_not_** install CRDs that are defined in the chart.

Workflow should be something like:

## Install

```shell
helm crdhub install mychart
```

Since  `mychart` requires a prometheus CRD but in this example prometheus is not installed yet, crdhub reaches out to the CRD Hub server requesting a CRD that will satisfy the kind and apiVersion: `prometheus.v1.monitoring.coreos.com`. The server will respond with the latest CRD that satisfies that requirement and installs it into the cluster.

Next it runs the install, removing any CRDs that have already been installed from the chart template.

## Upgrade CRD(s)

```shell
helm crdhub upgradecrds prometheus.monitoring.coreos.com ...
```

This will query the CRD Hub server and find the latest CRD for the group-kinds listed.

```shell
helm crdhub upgradecrds --all
```

This will query the CRD Hub server for all installed group-kinds and install the latest CRD.
