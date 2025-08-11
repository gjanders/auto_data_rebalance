## Introduction

This application exists to automate the data rebalance process on indexer clusters. The data rebalance will only trigger if the search factor is currently met.

## Installation
This application only needs to be installed on a cluster manager, it is not useful on any other instance.

## Usage
```
[auto_data_rebalance://example]
threshold = 0.9
max_runtime = 60
target_index = _internal
searchable = True
debug = True
interval = 33 6 * * *
```

The above example would run the auto_data_rebalance everyday at 6:33AM on the _internal index only with a rebalance_threshold of 0.9, a maximum runtime of 60 minutes and in searchable mode
This example also enables all debug logging, a more typical use case might be:
```
[auto_data_rebalance://example]
threshold = 0.95
max_runtime = 60
interval = 33 6 * * *
```

searchable and debug will both default to False, the threshold if not specified will use 0.9 and runtime will not be limited unless specified.

If you are running Splunk 9.3 or newer you have a new option to consider, usage-based rebalancing:
[Usage based data rebalancing](https://help.splunk.com/en/splunk-enterprise/administer/manage-indexers-and-indexer-clusters/10.0/manage-the-indexer-cluster/rebalance-the-indexer-cluster#id_3f07f54d_f2d0_49e1_8b13_527d3e640007__Rebalance_indexer_cluster_data_based_on_search_usage)

This option will run based on cluster-usage, however, it does not have a searchable mode or a target index, it will rebalance the entire cluster.
To use this option use the parameter:

`usage_based = True`

If not set, this app will default to the standard data rebalance method.

I did find while using the usage_based endpoints that the excess buckets were not removed, or at least not removed as much as I expected, so this argument:

`excess_buckets = True`

Will run an excess bucket removal instead of a data rebalance, you can define 1 input for excess bucket removal and another for rebalancing the data. These operations will conflict so you will need to use different times of the day.
Note that a regular (non-usage) data rebalance does trigger an excess bucket removal.
The excess bucket argument will also respect the target_index setting if set.

## Troubleshooting
A log file is created in $SPLUNK_HOME/var/log/splunk/auto_data_rebalance.log

You can also enable debug logging if you need to troubleshoot the input

## Feedback?
Feel free to open an issue on github or use the contact author on the [SplunkBase link](TBA) and I will try to get back to you when possible, thanks!

## Release Notes
### 0.0.1
Initial version

