{% list tabs group=instructions %}

- Management console {#console}

  1. Log in to the [management console]({{ link-console-main }}) with the cloud administrator or owner account.

  1. On the left side of the screen, click the line with the name of the [cloud](../../resource-manager/concepts/resources-hierarchy.md#cloud) or [folder](../../resource-manager/concepts/resources-hierarchy.md#folder) for which you want to assign a role to a user group.

  1. At the top of the screen, go to the **{{ ui-key.yacloud.common.resource-acl.label_access-bindings }}** tab and click **{{ ui-key.yacloud.common.resource-acl.button_configure-access }}**. In the window that opens:

      1. Go to the **{{ ui-key.yacloud_org.pages.groups }}** tab and select the [group](../../organization/concepts/groups.md) you need or search by group name.

          You can also assign a role to one of the [system](../../iam/concepts/access-control/system-group.md) groups:

          * `All users in organization X`: The group includes all users in organization `X`.
          * `All users in federation N`: The group includes all users in federation `N`.

      1. Click ![plus](../../_assets/console-icons/plus.svg) **{{ ui-key.yacloud_components.acl.action.add-role }}** and select the [role](../../iam/concepts/access-control/roles.md) you want to assign to the group for the cloud or folder you selected earlier. You can assign multiple roles.

      1. Click **{{ ui-key.yacloud.common.save }}**.

- CLI {#cli}

  {% include [cli-install](../cli-install.md) %}

  1. Select a [role](../../iam/concepts/access-control/roles.md) from the [{{ yandex-cloud }} role reference](../../iam/roles-reference.md).
  1. Assign the role using this command:

     ```bash
     yc <service_name> <resource> add-access-binding <resource_name_or_ID> \
       --role <role_ID> \
       --subject group:<group_ID>
     ```

     Where:
     
     * `--role`: Role ID, e.g., `{{ roles-cloud-owner }}`.
     * `--subject group`: ID of the [group](../../organization/concepts/groups.md) the role is assigned to.

         To assign a role to one of the [system groups](../../iam/concepts/access-control/system-group.md), instead of `--subject`, use the `--organization-users <organization_ID>` or `--federation-users <federation_ID>` parameter. In the parameter, provide the ID of the [organization](../../organization/quickstart.md) or [identity federation](../../organization/concepts/add-federation.md), respectively, to all the users you want to assign the role to.
         
         You can also assign a role to a system group using the `--subject` parameter. To do this, provide in it the [subject](../../iam/concepts/access-control/index.md#subject) ID matching the selected system group.

     For example, assign the `resource-manager.viewer` role for the `my-cloud` [cloud](../../resource-manager/concepts/resources-hierarchy.md#folder):

     ```bash
     yc resource-manager cloud add-access-binding mycloud \
       --role resource-manager.viewer \
       --subject group:aje6o61dvog2********
     ```

- {{ TF }} {#tf}

  {% include [terraform-install](../../_includes/terraform-install.md) %}

  1. Add the resource parameters to the configuration file and specify the required [role](../../iam/concepts/access-control/roles.md) and [group](../../organization/concepts/groups.md):

     ```hcl
     resource "yandex_resourcemanager_cloud_iam_member" "admin" {
       cloud_id    = "<cloud_ID>"
       role        = "<role_ID>"
       member      = "group:<group_ID>"
     }
     ```

     Where:
     
     * `cloud_id`: [Cloud ID](../../resource-manager/operations/cloud/get-id.md). You can also assign a role within an individual [folder](../../resource-manager/concepts/resources-hierarchy.md#folder). To do this, specify `folder_id` instead of `cloud_id` and the required folder ID in the resource parameters.
     * `role`: Role to assign. This is a required parameter.
     * `member`: Group the role is assigned to. Use this format: `group:<group_ID>`. This is a required parameter.

         To assign a role to one of the [system groups](../../iam/concepts/access-control/system-group.md), specify the following in the `member` parameter:

         * `system:group:organization:<organization_ID>:users`: To assign a role to the `All users in organization X` system group.
         * `system:group:federation:<federation_ID>:users`: To assign a role to the `All users in federation N` system group.

     For more information about the `yandex_resourcemanager_cloud_iam_member` resource parameters, see [this {{ TF }} article]({{ tf-provider-resources-link }}/iam_service_account_iam_member).
  1. Create the resources:

     {% include [terraform-validate-plan-apply](../../_tutorials/_tutorials_includes/terraform-validate-plan-apply.md) %}

     This will create all the resources you need in the specified folder. You can check the new resource using the [management console]({{ link-console-main }}) or this [CLI](../../cli/) command:

     ```bash
     terraform plan
     ```

     If the configuration is correct, the terminal will display a list of the resources being created and their parameters. If the configuration contains any errors, {{ TF }} will point them out.
  1. Deploy the cloud resources.
     1. If the configuration does not contain any errors, run this command:

        ```bash
        terraform apply
        ```

     1. Confirm creating the resources: type `yes` in the terminal and press **Enter**.

     This will create all the resources you need in the specified folder. You can check the new resource using the [management console]({{ link-console-main }}) or this CLI command:

     ```bash
     yc resource-manager folder list-access-bindings <folder_name_or_ID>
     ```

- API {#api}

  Use the `updateAccessBindings` REST API method for the appropriate resource.
  1. Select a [role](../../iam/concepts/access-control/roles.md) from the [{{ yandex-cloud }} role reference](../../iam/roles-reference.md).
  1. Create the request body, e.g., in the `body.json` file. In the `action` property, enter `ADD`, and specify the `group` type and group ID under `subject`:

     **body.json:**

     ```json
     {
       "accessBindingDeltas": [{
         "action": "ADD",
         "accessBinding": {
           "roleId": "editor",
           "subject": {
             "id": "<group_ID>",
             "type": "group"
           }
         }
       }]
     }
     ```

  1. {% include [grant-role-folder-via-curl-step](../iam/grant-role-folder-via-curl-step.md) %}

  To learn how to assign a role for the respective resource, see:
  * [{#T}](../../iam/operations/sa/set-access-bindings.md).
  * [{#T}](../../resource-manager/operations/cloud/set-access-bindings.md).
  * [{#T}](../../resource-manager/operations/folder/set-access-bindings.md).

{% endlist %}