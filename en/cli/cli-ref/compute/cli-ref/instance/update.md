---
editable: false
sourcePath: en/_cli-ref/cli-ref/compute/cli-ref/instance/update.md
---

# yc compute instance update

Update the specified virtual machine instance

#### Command Usage

Syntax: 

`yc compute instance update <INSTANCE-NAME>|<INSTANCE-ID> [Flags...] [Global Flags...]`

#### Flags

| Flag | Description |
|----|----|
|`--id`|<b>`string`</b><br/>Instance id.|
|`--name`|<b>`string`</b><br/>Instance name.|
|`--async`|Display information about the operation in progress, without waiting for the operation to complete.|
|`--new-name`|<b>`string`</b><br/>A new name of the instance.|
|`--description`|<b>`string`</b><br/>Specifies a textual description of the instance.|
|`--labels`|<b>`key=value[,key=value...]`</b><br/>A list of label KEY=VALUE pairs to add. For example, to add two labels named 'foo' and 'bar', both with the value 'baz', use '--labels foo=baz,bar=baz'.|
|`--metadata`|<b>`key=value[,key=value...]`</b><br/>Metadata to be made available to the guest operating system running on the instance.<br/>See for details https://yandex.cloud/ru/docs/compute/concepts/vm-metadata|
|`--metadata-options`|<b>`PROPERTY=VALUE[,PROPERTY=VALUE...]`</b><br/>Options set the access mode to instance's metadata.<br/><br/>Possible property names:<br/><ul> <li><code>aws-v1-http-endpoint</code>:     Enables access to AWS flavored metadata (IMDSv1). Values: enabled, disabled</li> <li><code>aws-v1-http-token</code>:     Enables access to IAM credentials with AWS flavored metadata (IMDSv1). Values: enabled, disabled</li> <li><code>gce-http-endpoint</code>:     Enables access to GCE flavored metadata. Values: enabled, disabled</li> <li><code>gce-http-token</code>:     Enables access to IAM credentials with GCE flavored metadata. Values: enabled, disabled</li> </ul>|
|`--metadata-from-file`|<b>`key=value[,key=value...]`</b><br/>Same as --metadata except that the value for the entry will be read from a local file.|
|`--memory`|<b>`byteSize`</b><br/>Specifies how much memory instance should have.|
|`--cores`|<b>`int`</b><br/>Specifies how many CPU cores instance should have.|
|`--core-fraction`|<b>`int`</b><br/>If provided, specifies baseline performance for a core in percent.|
|`--gpus`|<b>`int`</b><br/>Specifies how many GPUs instance should have.|
|`--platform`|<b>`string`</b><br/>Specifies platform for the instance.|
|`--network-settings`|<b>`PROPERTY=VALUE[,PROPERTY=VALUE...]`</b><br/>Network settings for the instance.<br/><br/>Possible property names:<br/><ul> <li><code>type</code>:     Type of a network for virtual machine instance. Values: standard, software-accelerated</li> </ul>|
|`--serial-port-settings`|<b>`PROPERTY=VALUE[,PROPERTY=VALUE...]`</b><br/>Serial port settings for the instance.<br/><br/>Possible property names:<br/><ul> <li><code>ssh-authorization</code>:     Authorization mode in interactive serial port console via SSH. Values: instance_metadata, os_login</li> </ul>|
|`--preemptible`|Specifies whether instance preemptible or not|
|`--maintenance-policy`|<b>`string`</b><br/>Behaviour on maintenance events. Values: 'restart', 'migrate'|
|`--maintenance-grace-period`|<b>`duration`</b><br/>Time between notification via metadata service and maintenance.|
|`--placement-group-id`|<b>`string`</b><br/>Placement group to add the instance to (use an empty string to remove the instance from placement group).|
|`--placement-group-name`|<b>`string`</b><br/>Placement group to add the instance to (use an empty string to remove the instance from placement group).|
|`--placement-group-partition`|<b>`int`</b><br/>Specifies placement group partition for the instance.|
|`--host-group-id`|<b>`string`</b><br/>Host group to place the instance to (to clear affinity rules use --no-host-affinity-rules).|
|`--host-group-name`|<b>`string`</b><br/>Host group to place the instance to (to clear affinity rules use --no-host-affinity-rules).|
|`--host-id`|<b>`string`</b><br/>Host to place the instance to (to remove all affinity rules use --no-host-affinity-rules).|
|`--no-host-affinity-rules`|Clear host affinity rules|
|`--reserved-instance-pool-id`|<b>`string`</b><br/>Link instance to specified reserved instance pool.|
|`--reserved-instance-pool-name`|<b>`string`</b><br/>Link instance to specified reserved instance pool.|
|`--service-account-id`|<b>`string`</b><br/>Service account, which token can be obtained inside VM from metadata service.|
|`--service-account-name`|<b>`string`</b><br/>Service account, which token can be obtained inside VM from metadata service.|

#### Global Flags

| Flag | Description |
|----|----|
|`--profile`|<b>`string`</b><br/>Set the custom configuration file.|
|`--debug`|Debug logging.|
|`--debug-grpc`|Debug gRPC logging. Very verbose, used for debugging connection problems.|
|`--no-user-output`|Disable printing user intended output to stderr.|
|`--retry`|<b>`int`</b><br/>Enable gRPC retries. By default, retries are enabled with maximum 5 attempts.<br/>Pass 0 to disable retries. Pass any negative value for infinite retries.<br/>Even infinite retries are capped with 2 minutes timeout.|
|`--cloud-id`|<b>`string`</b><br/>Set the ID of the cloud to use.|
|`--folder-id`|<b>`string`</b><br/>Set the ID of the folder to use.|
|`--folder-name`|<b>`string`</b><br/>Set the name of the folder to use (will be resolved to id).|
|`--endpoint`|<b>`string`</b><br/>Set the Cloud API endpoint (host:port).|
|`--token`|<b>`string`</b><br/>Set the OAuth token to use.|
|`--impersonate-service-account-id`|<b>`string`</b><br/>Set the ID of the service account to impersonate.|
|`--no-browser`|Disable opening browser for authentication.|
|`--format`|<b>`string`</b><br/>Set the output format: text (default), yaml, json, json-rest.|
|`--jq`|<b>`string`</b><br/>Query to select values from the response using jq syntax|
|`-h`,`--help`|Display help for the command.|
