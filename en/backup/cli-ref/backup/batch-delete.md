---
editable: false
sourcePath: en/_cli-ref/cli-ref/backup/cli-ref/backup/batch-delete.md
---

# yc backup backup batch-delete

Batch deletion of all backups from arhcive

#### Command Usage

Syntax: 

`yc backup backup batch-delete [Flags...] [Global Flags...]`

#### Flags

| Flag | Description |
|----|----|
|`--async`|Display information about the operation in progress, without waiting for the operation to complete.|
|`--archive-id`|<b>`string`</b><br/>Specified archive id to delete all backups from this archive|
|`--instance-id`|<b>`string`</b><br/>Specified instance id to delete all backups from archive linked to given policy and instance, --policy-id must be specified with this param|
|`--policy-id`|<b>`string`</b><br/>Specified policy id to delete all backups from archive linked to given policy and instance, --instance-id must be specified with this param|

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
