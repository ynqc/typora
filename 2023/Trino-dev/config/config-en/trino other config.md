##### exchange-manager.properties

exchange-manager.name=filesystem
 exchange.base-directory=/tmp/trino-local-file-system-exchange-manager

|                                  |                                                              |
| -------------------------------- | ------------------------------------------------------------ |
| exchange-manager.name=filesystem | To configure an exchange manager, create a new `etc/exchange-manager.properties` configuration file on the coordinator and all worker nodes. In this file, set the `exchange-manager.name` configuration property to `filesystem` or `hdfs`, and set additional configuration properties as needed for your storage solution.The following table lists the available configuration properties for `exchange-manager.properties`, their default values, and which filesystem(s) the property may be configured for: |
| exchange.base-directories        | Comma-separated list of URI locations that the exchange manager uses to store spooling data. |



##### log.properties

io.trino=INFO



##### node.properties

node.environment=production
node.data-dir=/data/trino
plugin.dir=/usr/lib/trino/plugin



The node properties file, `etc/node.properties`, contains configuration specific to each node. A *node* is a single installed instance of Trino on a machine. This file is typically created by the deployment system when Trino is first installed. The following is a minimal `etc/node.properties`:

node.environment=production
node.id=ffffffff-ffff-ffff-ffff-ffffffffffff
node.data-dir=/var/trino/data

The above properties are described below:

- `node.environment`: The name of the environment. All Trino nodes in a cluster must have the same environment name. The name must start with a lowercase alphanumeric character and only contain lowercase alphanumeric or underscore (`_`) characters.
- `node.id`: The unique identifier for this installation of Trino. This must be unique for every node. This identifier should remain consistent across reboots or upgrades of Trino. If running multiple installations of Trino on a single machine (i.e. multiple nodes on the same machine), each installation must have a unique identifier. The identifier must start with an alphanumeric character and only contain alphanumeric, `-`, or `_` characters.
- `node.data-dir`: The location (filesystem path) of the data directory. Trino stores logs and other data here.



|                                  |                                                              |      |
| -------------------------------- | ------------------------------------------------------------ | ---- |
| node.environment=production      |                                                              |      |
| node.data-dir=/data/trino        |                                                              |      |
| plugin.dir=/usr/lib/trino/plugin | By default, the plugin directory is the `plugin` directory relative to the directory in which Trino is installed, but it is configurable using the configuration variable `plugin.dir`. In order for Trino to pick up the new plugin, you must restart Trino.Plugins must be installed on all nodes in the Trino cluster (coordinator and workers). |      |

