---
editable: false
sourcePath: en/_api-ref-grpc/compute/v1/api-ref/grpc/HostGroup/update.md
---

# Compute Cloud API, gRPC: HostGroupService.Update

Updates the specified host group.

## gRPC request

**rpc Update ([UpdateHostGroupRequest](#yandex.cloud.compute.v1.UpdateHostGroupRequest)) returns ([operation.Operation](#yandex.cloud.operation.Operation))**

## UpdateHostGroupRequest {#yandex.cloud.compute.v1.UpdateHostGroupRequest}

```json
{
  "host_group_id": "string",
  "update_mask": "google.protobuf.FieldMask",
  "name": "string",
  "description": "string",
  "labels": "map<string, string>",
  "maintenance_policy": "MaintenancePolicy",
  "scale_policy": {
    // Includes only one of the fields `fixed_scale`
    "fixed_scale": {
      "size": "int64"
    }
    // end of the list of possible fields
  }
}
```

#|
||Field | Description ||
|| host_group_id | **string**

Required field. ID of the host group to update.
To get the host group ID, use an [HostGroupService.List](/docs/compute/api-ref/grpc/HostGroup/list#List) request. ||
|| update_mask | **[google.protobuf.FieldMask](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/field-mask)**

Field mask that specifies which fields of the HostGroup resource are going to be updated. ||
|| name | **string**

Name of the group. ||
|| description | **string**

Description of the group. ||
|| labels | **object** (map<**string**, **string**>)

Resource labels as `key:value` pairs.

The existing set of `labels` is completely replaced by the provided set. ||
|| maintenance_policy | enum **MaintenancePolicy**

Behaviour on maintenance events

- `MAINTENANCE_POLICY_UNSPECIFIED`
- `RESTART`: Restart instance to move it to another host during maintenance
- `MIGRATE`: Use live migration to move instance to another host during maintenance ||
|| scale_policy | **[ScalePolicy](#yandex.cloud.compute.v1.ScalePolicy)**

Scale policy. Only fixed number of hosts are supported at this moment. ||
|#

## ScalePolicy {#yandex.cloud.compute.v1.ScalePolicy}

#|
||Field | Description ||
|| fixed_scale | **[FixedScale](#yandex.cloud.compute.v1.ScalePolicy.FixedScale)**

Includes only one of the fields `fixed_scale`. ||
|#

## FixedScale {#yandex.cloud.compute.v1.ScalePolicy.FixedScale}

#|
||Field | Description ||
|| size | **int64** ||
|#

## operation.Operation {#yandex.cloud.operation.Operation}

```json
{
  "id": "string",
  "description": "string",
  "created_at": "google.protobuf.Timestamp",
  "created_by": "string",
  "modified_at": "google.protobuf.Timestamp",
  "done": "bool",
  "metadata": {
    "host_group_id": "string"
  },
  // Includes only one of the fields `error`, `response`
  "error": "google.rpc.Status",
  "response": {
    "id": "string",
    "folder_id": "string",
    "created_at": "google.protobuf.Timestamp",
    "name": "string",
    "description": "string",
    "labels": "map<string, string>",
    "zone_id": "string",
    "status": "Status",
    "type_id": "string",
    "maintenance_policy": "MaintenancePolicy",
    "scale_policy": {
      // Includes only one of the fields `fixed_scale`
      "fixed_scale": {
        "size": "int64"
      }
      // end of the list of possible fields
    }
  }
  // end of the list of possible fields
}
```

An Operation resource. For more information, see [Operation](/docs/api-design-guide/concepts/operation).

#|
||Field | Description ||
|| id | **string**

ID of the operation. ||
|| description | **string**

Description of the operation. 0-256 characters long. ||
|| created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Creation timestamp. ||
|| created_by | **string**

ID of the user or service account who initiated the operation. ||
|| modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

The time when the Operation resource was last modified. ||
|| done | **bool**

If the value is `false`, it means the operation is still in progress.
If `true`, the operation is completed, and either `error` or `response` is available. ||
|| metadata | **[UpdateHostGroupMetadata](#yandex.cloud.compute.v1.UpdateHostGroupMetadata)**

Service-specific metadata associated with the operation.
It typically contains the ID of the target resource that the operation is performed on.
Any method that returns a long-running operation should document the metadata type, if any. ||
|| error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**

The error result of the operation in case of failure or cancellation.

Includes only one of the fields `error`, `response`.

The operation result.
If `done == false` and there was no failure detected, neither `error` nor `response` is set.
If `done == false` and there was a failure detected, `error` is set.
If `done == true`, exactly one of `error` or `response` is set. ||
|| response | **[HostGroup](#yandex.cloud.compute.v1.HostGroup)**

The normal response of the operation in case of success.
If the original method returns no data on success, such as Delete,
the response is [google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty).
If the original method is the standard Create/Update,
the response should be the target resource of the operation.
Any method that returns a long-running operation should document the response type, if any.

Includes only one of the fields `error`, `response`.

The operation result.
If `done == false` and there was no failure detected, neither `error` nor `response` is set.
If `done == false` and there was a failure detected, `error` is set.
If `done == true`, exactly one of `error` or `response` is set. ||
|#

## UpdateHostGroupMetadata {#yandex.cloud.compute.v1.UpdateHostGroupMetadata}

#|
||Field | Description ||
|| host_group_id | **string**

ID of the host group that is being updated. ||
|#

## HostGroup {#yandex.cloud.compute.v1.HostGroup}

Represents group of dedicated hosts

#|
||Field | Description ||
|| id | **string**

ID of the group. ||
|| folder_id | **string**

ID of the folder that the group belongs to. ||
|| created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Creation timestamp in [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) text format. ||
|| name | **string**

Name of the group. The name is unique within the folder. ||
|| description | **string**

Description of the group. ||
|| labels | **object** (map<**string**, **string**>)

Resource labels as `key:value` pairs. ||
|| zone_id | **string**

Availability zone where all dedicated hosts are allocated. ||
|| status | enum **Status**

Status of the group.

- `STATUS_UNSPECIFIED`
- `CREATING`
- `READY`
- `UPDATING`
- `DELETING` ||
|| type_id | **string**

ID of host type. Resources provided by each host of the group. ||
|| maintenance_policy | enum **MaintenancePolicy**

Behaviour on maintenance events.

- `MAINTENANCE_POLICY_UNSPECIFIED`
- `RESTART`: Restart instance to move it to another host during maintenance
- `MIGRATE`: Use live migration to move instance to another host during maintenance ||
|| scale_policy | **[ScalePolicy](#yandex.cloud.compute.v1.ScalePolicy2)**

Scale policy. Only fixed number of hosts are supported at this moment. ||
|#

## ScalePolicy {#yandex.cloud.compute.v1.ScalePolicy2}

#|
||Field | Description ||
|| fixed_scale | **[FixedScale](#yandex.cloud.compute.v1.ScalePolicy.FixedScale2)**

Includes only one of the fields `fixed_scale`. ||
|#

## FixedScale {#yandex.cloud.compute.v1.ScalePolicy.FixedScale2}

#|
||Field | Description ||
|| size | **int64** ||
|#