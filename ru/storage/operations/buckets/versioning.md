---
title: Управление версионированием бакета в {{ objstorage-full-name }}
description: Следуя данной инструкции, вы научитесь управлять версионированием бакета в {{ objstorage-name }}.
---

# Управление версионированием бакета

[Версионирование](../../concepts/versioning.md) бакета — это возможность хранить историю объекта с помощью версий.

{% note info %}

Операция включения необратима: отключить версионирование нельзя, можно только приостановить создание новых версий. После приостановки версионирования новые объекты будут сохраняться с версией `null`.

{% endnote %}

Включить версионирование бакета:

{% list tabs group=instructions %}

- Консоль управления {#console}

  1. В [консоли управления]({{ link-console-main }}) в списке сервисов выберите **{{ ui-key.yacloud.iam.folder.dashboard.label_storage }}**.
  1. Нажмите на имя нужного бакета.
  1. На панели слева выберите ![image](../../../_assets/console-icons/wrench.svg) **{{ ui-key.yacloud.storage.bucket.switch_settings }}**.
  1. Выберите вкладку **{{ ui-key.yacloud.storage.bucket.switch_versioning }}**.
  1. Чтобы включить или выключить версионирование, используйте опцию **{{ ui-key.yacloud.storage.bucket.versioning.field_status }}**.
  1. Нажмите **{{ ui-key.yacloud.storage.bucket.settings.button_save }}**.

- {{ yandex-cloud }} CLI {#cli}

  {% include [cli-install](../../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

  1. Посмотрите описание команды CLI для редактирования ACL бакета:

     ```bash
     yc storage bucket update --help
     ```  

  1. Выполните следующую команду:

     ```bash
     yc storage bucket update --name <имя_бакета> --versioning versioning-enabled
     ```

     Результат:

     ```text
     name: my-bucket
     folder_id: csgeoelk7fl15********
     default_storage_class: STANDARD
     versioning: VERSIONING_ENABLED
     max_size: "10737418240"
     acl: {}
     created_at: "2022-12-14T08:42:16.273717Z"
     ```

- AWS CLI {#aws-cli}

  Если у вас еще нет AWS CLI, [установите и сконфигурируйте его](../../tools/aws-cli.md).

  Выполните следующую команду:

  ```bash
  aws --endpoint https://{{ s3-storage-host }} \
    s3api put-bucket-versioning \
    --bucket <имя_бакета> \
    --versioning-configuration 'Status=Enabled'
  ```

- {{ TF }} {#tf}

  {% include [terraform-role](../../../_includes/storage/terraform-role.md) %}

  {% include [terraform-install](../../../_includes/terraform-install.md) %}

  Получите [статические ключи доступа](../../../iam/operations/authentication/manage-access-keys.md#create-access-key) — секретный ключ и идентификатор ключа, используемые для аутентификации в {{ objstorage-short-name }}.

  {% include [terraform-iamtoken-note](../../../_includes/storage/terraform-iamtoken-note.md) %}

  Опишите в конфигурационном файле параметры ресурсов, которые необходимо создать:

  ```hcl
  resource "yandex_iam_service_account" "sa" {
    name = "<имя_сервисного_аккаунта>"
  }

  // Назначение роли сервисному аккаунту
  resource "yandex_resourcemanager_folder_iam_member" "sa-admin" {
    folder_id = "<идентификатор_каталога>"
    role      = "storage.admin"
    member    = "serviceAccount:${yandex_iam_service_account.sa.id}"
  }

  // Создание статического ключа доступа
  resource "yandex_iam_service_account_static_access_key" "sa-static-key" {
    service_account_id = yandex_iam_service_account.sa.id
    description        = "static access key for object storage"
  }

  resource "yandex_storage_bucket" "b" {
    bucket     = "<имя_бакета>"
    access_key = yandex_iam_service_account_static_access_key.sa-static-key.access_key
    secret_key = yandex_iam_service_account_static_access_key.sa-static-key.secret_key
    acl        = "private"

    versioning {
      enabled = true
    }
  }
  ```

  Где:

  * `bucket` — имя бакета. Обязательный параметр.
  * `access_key` — идентификатор статического ключа доступа.
  * `secret_key` — значение секретного ключа доступа.
  * `acl` — применяемая политика ACL. Значение по умолчанию `private`. Необязательный параметр.
  * `versioning` — управление версионированием бакета:
    * `enabled` — включает версионирование бакета. Необязательный параметр.

- API {#api}

  Чтобы управлять версионированием бакета, воспользуйтесь методом REST API [update](../../api-ref/Bucket/update.md) для ресурса [Bucket](../../api-ref/Bucket/index.md), вызовом gRPC API [BucketService/Update](../../api-ref/grpc/Bucket/update.md) или методом S3 API [putBucketVersioning](../../s3/api-ref/bucket/putBucketVersioning.md).

{% endlist %}
