---
editable: false
sourcePath: en/_api-ref/serverless/functions/v1/functions/api-ref/Function/list.md
---

# Cloud Functions Service, REST: Function.List

Retrieves the list of functions in the specified folder.

## HTTP request

```
GET https://serverless-functions.{{ api-host }}/functions/v1/functions
```

## Query parameters {#yandex.cloud.serverless.functions.v1.ListFunctionsRequest}

#|
||Field | Description ||
|| folderId | **string**

Required field. ID of the folder to list functions in.

To get a folder ID make a [yandex.cloud.resourcemanager.v1.FolderService.List](/docs/resource-manager/api-ref/Folder/list#List) request. ||
|| pageSize | **string** (int64)

The maximum number of results per page to return. If the number of available
results is larger than `pageSize`, the service returns a [ListFunctionsResponse.nextPageToken](#yandex.cloud.serverless.functions.v1.ListFunctionsResponse)
that can be used to get the next page of results in subsequent list requests.

Default value: 100. ||
|| pageToken | **string**

Page token. To get the next page of results, set `pageToken` to the
[ListFunctionsResponse.nextPageToken](#yandex.cloud.serverless.functions.v1.ListFunctionsResponse) returned by a previous list request. ||
|| filter | **string**

A filter expression that filters functions listed in the response.

The expression must specify:
1. The field name. Currently filtering can only be applied to the [Function.name](#yandex.cloud.serverless.functions.v1.Function) field.
2. An `=` operator.
3. The value in double quotes (`"`). Must be 3-63 characters long and match the regular expression `[a-z][-a-z0-9]{1,61}[a-z0-9]`.
Example of a filter: `name="my-function"`. ||
|#

## Response {#yandex.cloud.serverless.functions.v1.ListFunctionsResponse}

**HTTP Code: 200 - OK**

```json
{
  "functions": [
    {
      "id": "string",
      "folderId": "string",
      "createdAt": "string",
      "name": "string",
      "description": "string",
      "labels": "object",
      "httpInvokeUrl": "string",
      "status": "string"
    }
  ],
  "nextPageToken": "string"
}
```

#|
||Field | Description ||
|| functions[] | **[Function](#yandex.cloud.serverless.functions.v1.Function)**

List of functions in the specified folder. ||
|| nextPageToken | **string**

Token for getting the next page of the list. If the number of results is greater than
the specified [ListFunctionsRequest.pageSize](#yandex.cloud.serverless.functions.v1.ListFunctionsRequest), use `nextPageToken` as the value
for the [ListFunctionsRequest.pageToken](#yandex.cloud.serverless.functions.v1.ListFunctionsRequest) parameter in the next list request.

Each subsequent page will have its own `nextPageToken` to continue paging through the results. ||
|#

## Function {#yandex.cloud.serverless.functions.v1.Function}

A serverless function. For details about the concept, see [Functions](/docs/functions/concepts/function).

#|
||Field | Description ||
|| id | **string**

ID of the function. Generated at creation time. ||
|| folderId | **string**

ID of the folder that the function belongs to. ||
|| createdAt | **string** (date-time)

Creation timestamp for the function.

String in [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) text format. The range of possible values is from
`0001-01-01T00:00:00Z` to `9999-12-31T23:59:59.999999999Z`, i.e. from 0 to 9 digits for fractions of a second.

To work with values in this field, use the APIs described in the
[Protocol Buffers reference](https://developers.google.com/protocol-buffers/docs/reference/overview).
In some languages, built-in datetime utilities do not support nanosecond precision (9 digits). ||
|| name | **string**

Name of the function. The name is unique within the folder. ||
|| description | **string**

Description of the function. ||
|| labels | **object** (map<**string**, **string**>)

Function labels as `key:value` pairs. ||
|| httpInvokeUrl | **string**

URL that needs to be requested to invoke the function. ||
|| status | **enum** (Status)

Status of the function.

- `STATUS_UNSPECIFIED`
- `CREATING`: Function is being created.
- `ACTIVE`: Function is ready to be invoked.
- `DELETING`: Function is being deleted.
- `ERROR`: Function failed. ||
|#