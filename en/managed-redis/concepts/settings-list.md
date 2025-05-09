# {{ VLK }} settings

For {{ mrd-name }} clusters, you can configure {{ VLK }} settings.

The label next to the setting name shows which interface can be used to configure this setting: the management console, CLI, API, or {{ TF }}. The {{ tag-all }} label means that all of the above interfaces are supported.

Depending on the selected interface, the same setting will be represented differently. For example, **Maxmemory policy** in the management console is the same as:

- `maxmemory_policy` in the gRPC API and {{ TF }}
- `maxmemoryPolicy` in the REST API

## DBMS settings {#dbms-settings}

You can use the following settings:

{% include [mrd-dbms-settings](../../_includes/mdb/mrd-dbms-settings.md) %}
