##### exchange-manager.properties

exchange-manager.name=filesystem
 exchange.base-directory=/tmp/trino-local-file-system-exchange-manager

|                                  |                                                              |                                                              |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| exchange-manager.name=filesystem | To configure an exchange manager, create a new `etc/exchange-manager.properties` configuration file on the coordinator and all worker nodes. In this file, set the `exchange-manager.name` configuration property to `filesystem` or `hdfs`, and set additional configuration properties as needed for your storage solution.The following table lists the available configuration properties for `exchange-manager.properties`, their default values, and which filesystem(s) the property may be configured for: | 要配置exchange manager，请在协调器和所有工作节点上创建新的etc/exchange-manager.properties配置文件。在该文件中，将exchange-manager.name配置属性设置为文件系统或hdfs，并根据存储解决方案的需要设置其他配置财产。<br/> |
| exchange.base-directories        | Comma-separated list of URI locations that the exchange manager uses to store spooling data. | 交换管理器用于存储后台处理数据的URI位置的逗号分隔列表。      |
|                                  |                                                              |                                                              |



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

上述财产如下所述：
node.environment：环境的名称。集群中的所有Trino节点必须具有相同的环境名称。名称必须以小写字母数字字符开头，并且只能包含小写字母数字或下划线（_）字符。
node.id:Trino安装的唯一标识符。这对于每个节点都必须是唯一的。此标识符应在Trino的重新启动或升级过程中保持一致。如果在一台机器上运行Trino的多个安装（即在同一机器上运行多个节点），则每个安装都必须具有唯一的标识符。标识符必须以字母数字字符开头，并且只能包含字母数字、-或_字符。
node.data-dr：数据目录的位置（文件系统路径）。Trino在此处存储日志和其他数据。

|                                  |                                                              |      |
| -------------------------------- | ------------------------------------------------------------ | ---- |
| node.environment=production      |                                                              |      |
| node.data-dir=/data/trino        |                                                              |      |
| plugin.dir=/usr/lib/trino/plugin | By default, the plugin directory is the `plugin` directory relative to the directory in which Trino is installed, but it is configurable using the configuration variable `plugin.dir`. In order for Trino to pick up the new plugin, you must restart Trino.Plugins must be installed on all nodes in the Trino cluster (coordinator and workers). |      |

