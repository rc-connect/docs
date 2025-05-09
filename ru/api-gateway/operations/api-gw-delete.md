---
title: Удалить API-шлюз
description: Следуя данной инструкции, вы сможете удалить API-шлюз.
---

# Удалить API-шлюз

{% list tabs group=instructions %}

- Консоль управления {#console}

  Чтобы удалить [API-шлюз](../concepts/index.md):
  1. В [консоли управления]({{ link-console-main }}) перейдите в [каталог](../../resource-manager/concepts/resources-hierarchy.md#folder), в котором необходимо удалить API-шлюз.
  1. В списке сервисов выберите **{{ ui-key.yacloud.iam.folder.dashboard.label_api-gateway }}**.
  1. В строке с API-шлюзом нажмите кнопку ![image](../../_assets/console-icons/ellipsis.svg) и выберите **{{ ui-key.yacloud.common.delete }}**.
  1. В открывшемся окне нажмите кнопку **{{ ui-key.yacloud.common.delete }}**.

- CLI {#cli}

  Чтобы удалить [API-шлюз](../concepts/index.md), выполните команду:

  ```bash
  yc serverless api-gateway delete --id <идентификатор_API-шлюза>
  ```

  Результат:

  ```text
  done (18s)
  ```

- {{ TF }} {#tf}

  {% include [terraform-definition](../../_tutorials/_tutorials_includes/terraform-definition.md) %}

  {% include [terraform-install](../../_includes/terraform-install.md) %}

  Чтобы удалить [API-шлюз](../concepts/index.md), созданный с помощью {{ TF }}:
  1. Откройте файл конфигураций {{ TF }} и удалите фрагмент с описанием API-шлюза.

     Пример описания API-шлюза в конфигурации {{ TF }}:

     ```hcl
     ...
     resource "yandex_api_gateway" "test-api-gateway" {
       name        = "some_name"
       description = "any description"
       labels      = {
         label       = "label"
         empty-label = ""
       }
       spec = <<-EOT
         openapi: "3.0.0"
         info:
           version: 1.0.0
           title: Test API
         paths:
           /hello:
             get:
               summary: Say hello
               operationId: hello
               parameters:
                 - name: user
                   in: query
                   description: User name to appear in greetings.
                   required: false
                   schema:
                     type: string
                     default: 'world'
               responses:
                 '200':
                   description: Greeting.
                   content:
                     'text/plain':
                       schema:
                         type: "string"
               x-yc-apigateway-integration:
                 type: dummy
                 http_code: 200
                 http_headers:
                   'Content-Type': "text/plain"
                 content:
                   'text/plain': "Hello again, {user}!\n"
       EOT
     }
     ...
     ```

  1. В командной строке перейдите в папку, где расположен файл конфигурации {{ TF }}.
  1. Проверьте конфигурацию командой:

     ```bash
     terraform validate
     ```

     Если конфигурация является корректной, появится сообщение:

     ```text
     Success! The configuration is valid.
     ```

  1. Выполните команду:

     ```bash
     terraform plan
     ```

     В терминале будет выведен список ресурсов с параметрами. На этом этапе изменения не будут внесены. Если в конфигурации есть ошибки, {{ TF }} на них укажет.
  1. Примените изменения конфигурации:

     ```bash
     terraform apply
     ```

  1. Подтвердите изменения: введите в терминал слово `yes` и нажмите **Enter**.

     Проверить изменения можно в [консоли управления]({{ link-console-main }}) или с помощью команд [CLI](../../cli/):

     ```bash
     yc serverless api-gateway list
     ```

- API {#api}

  Чтобы удалить [API-шлюз](../concepts/index.md), воспользуйтесь методом REST API [delete](../apigateway/api-ref/ApiGateway/delete.md) для ресурса [ApiGateway](../apigateway/api-ref/ApiGateway/index.md) или вызовом gRPC API [ApiGatewayService/Delete](../apigateway/api-ref/grpc/ApiGateway/delete.md).

- {{ yandex-cloud }} Toolkit {#yc-toolkit}

  Удалить [API-шлюз](../concepts/index.md) можно с помощью [плагина {{ yandex-cloud }} Toolkit](https://github.com/yandex-cloud/ide-plugin-jetbrains) для семейства IDE на [платформе IntelliJ](https://www.jetbrains.com/ru-ru/opensource/idea/) от [JetBrains](https://www.jetbrains.com/).

{% endlist %}