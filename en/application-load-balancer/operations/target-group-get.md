---
title: How to get information about a target group in {{ alb-full-name }}
---

# Getting information about a target group

{% list tabs group=instructions %}

- Management console {#console}

  1. In the [management console]({{ link-console-main }}), select your [target group](../concepts/target-group.md) folder.
  1. From the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_application-load-balancer }}**.
  1. In the left-hand panel, select ![image](../../_assets/console-icons/target.svg) **{{ ui-key.yacloud.alb.label_target-groups }}**.
  1. Select the target group.
  1. You will see your target group details on the **{{ ui-key.yacloud.common.overview }}** page.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  1. See the description of the CLI command for getting information about a [target group](../concepts/target-group.md):

      ```bash
      yc alb target-group get --help
      ```

  1. To get information about the target group, run this command with its ID or name specified:

      ```bash
      yc alb target-group get <target_group_name>
      ```

      Result:

      ```text
      id: ds7cjrs15ero********
      name: alb-tg
      folder_id: b1geoelk7fld********
      targets:
        - ip_address: 192.168.3.34
          subnet_id: b0cqh32phmc1********
        - ip_address: 192.168.2.4
          subnet_id: e2ltekraekpc********
        - ip_address: 192.168.1.10
          subnet_id: e9bns2fv233c********
      created_at: "2023-10-24T12:21:09.159841076Z"
      ```

- {{ TF }} {#tf}

  {% include [terraform-definition](../../_tutorials/_tutorials_includes/terraform-definition.md) %}

  {% include [terraform-install](../../_includes/terraform-install.md) %}

  To get information about a [target group](../concepts/target-group.md) using {{ TF }}:

  1. Add `data` and `output` sections to the {{ TF }} configuration file:

      ```hcl
      data "yandex_alb_target_group" "my_tg" {
        target_group_id = "<target_group_ID>"
      }

      output "target_group" {
        value = data.yandex_alb_target_group.my_tg.target
      }
      ```

      Where:

      * `data "yandex_alb_target_group"`: Description of the target group as a data source:
        * `target_group_id`: Target group ID.
      * `output "target_group"`: Output variable with target group resource information:
        * `value`: Returned value.

      You can replace `target` with another variable to get the information you need. For more information about `yandex_alb_target_group` data source variables, see the [provider documentation]({{ tf-provider-datasources-link }}/alb_target_group).

  1. Create the resources:

      {% include [terraform-validate-plan-apply](../../_tutorials/_tutorials_includes/terraform-validate-plan-apply.md) %}

      {{ TF }} will create the required resources and display their output variables. To check the results, run this command:

      ```bash
      terraform output
      ```

      Result:

      ```text
      target_group = tolist([
        {
          "ip_address" = "10.129.0.29"
          "private_ipv4_address" = false
          "subnet_id" = "e2lfebujacgf********"
        },
      ])
      ```

- API {#api}

  To get [target group](../concepts/target-group.md) details, use the [get](../api-ref/TargetGroup/get.md) REST API method for the [TargetGroup](../api-ref/TargetGroup/index.md) resource or the [TargetGroupService/Get](../api-ref/grpc/TargetGroup/get.md) gRPC API call.

{% endlist %}
