---
editable: false
sourcePath: en/_cli-ref/cli-ref/managed-greenplum/cli-ref/cluster/index.md
---

# yc managed-greenplum cluster

Manage Greenplum clusters

#### Command Usage

Syntax: 

`yc managed-greenplum cluster <command>`

Aliases: 

- `clusters`

#### Command Tree

- [yc managed-greenplum cluster add-labels](add-labels.md) — Add labels to Greenplum cluster
- [yc managed-greenplum cluster backup](backup.md) — Create a backup for the specified Greenplum cluster
- [yc managed-greenplum cluster create](create.md) — Create Greenplum cluster
- [yc managed-greenplum cluster delete](delete.md) — Delete the specified Greenplum cluster
- [yc managed-greenplum cluster expand](expand.md) — Expands the specified Greenplum cluster
- [yc managed-greenplum cluster get](get.md) — Show information about the specified Greenplum cluster
- [yc managed-greenplum cluster list](list.md) — List Greenplum clusters
- [yc managed-greenplum cluster list-backups](list-backups.md) — List available backups for the specified Greenplum cluster
- [yc managed-greenplum cluster list-logs](list-logs.md) — Retrieves logs for the specified Greenplum cluster
- [yc managed-greenplum cluster list-operations](list-operations.md) — List operations for the specified Greenplum cluster
- [yc managed-greenplum cluster move](move.md) — Move the specified Greenplum cluster into the folder
- [yc managed-greenplum cluster remove-labels](remove-labels.md) — Remove labels from Greenplum cluster
- [yc managed-greenplum cluster reschedule-maintenance](reschedule-maintenance.md) — Reschedule maintenance for the specified Greenplum cluster
- [yc managed-greenplum cluster restore](restore.md) — Restore Greenplum cluster
- [yc managed-greenplum cluster start](start.md) — Start the specified Greenplum cluster
- [yc managed-greenplum cluster stop](stop.md) — Stop the specified Greenplum cluster
- [yc managed-greenplum cluster update](update.md) — Update the specified Greenplum cluster
- [yc managed-greenplum cluster update-config](update-config.md) — Update the specified Greenplum cluster config

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
