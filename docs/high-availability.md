# Configuring High Availability

The Cartographer Conventions controller supports high availability following an active/passive model based on the leader election strategy. Since only one instance performs work at any given time, one replica for each Pod is enough. The leader election strategy is enabled by default.
