---
title: Составная (multipart) загрузка объекта в бакет в {{ objstorage-full-name }}
description: Следуя данной инструкции, вы сможете загрузить объект в бакет в {{ objstorage-name }} по частям.
---

# Составная (multipart) загрузка объекта

При загрузке [объекта](../../concepts/object.md) в [бакет](../../concepts/bucket.md) {{ objstorage-name }} может произойти сетевой сбой, в результате которого загрузка прервется и файл придется загружать заново. Чем больше размер загружаемого файла, тем дольше длится загрузка и выше риск возникновения сетевых ошибок.

Поэтому большие файлы эффективнее загружать в бакет частями с помощью [составной (multipart) загрузки](../../concepts/multipart.md). В случае сетевого сбоя при загрузке части файла повторно загружать потребуется только одну эту составную часть, а не весь файл.

Чтобы загрузить в бакет объект по частям:

## Разбейте исходный файл на части {#split}

На вашем компьютере разбейте исходный файл на части нужных размеров. Например, это можно сделать с помощью команды `split`:

```bash
split -b 100M ./video.mp4 part.
```

Где:

* `100M` — размер частей, на которые разбивается файл. В примере задан размер `100 МБ`.

    Суффикс `M` означает, что размер задан в мегабайтах. Вы также можете использовать суффиксы `K` и `G`, чтобы задать размер в килобайтах и гигабайтах соответственно.
* `./video.mp4` — путь к исходному файлу, который нужно разбить на части, чтобы потом загрузить в бакет.
* `part.` — префикс имен файлов, которые будут присваиваться частям.

Результат:

```text
% ls -l
total 822368
-rw-r--r--@ 1 User  group  104857600 Dec 20 22:56 part.aa
-rw-r--r--@ 1 User  group  104857600 Dec 20 22:56 part.ab
-rw-r--r--@ 1 User  group     808601 Dec 20 22:56 part.ab
-rwx------@ 1 User  group  210523801 Feb 24  2023 video.mp4
```

В результате выполнения команды `split` файл `video.mp4` разбит на три части: `part.aa`, `part.ab` и `part.ab`.

## Создайте составную загрузку в бакете {#create-multipart-upload}

{% list tabs group=instructions %}

- {{ yandex-cloud }} CLI {#cli}

  {% include [cli-install](../../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

  1. Посмотрите описание команды CLI для создания составной загрузки объекта в бакет:

      ```bash
      yc storage s3api create-multipart-upload --help
      ```

  1. Получите список бакетов в каталоге по умолчанию:

      ```bash
      yc storage bucket list
      ```

      Результат:

      ```text
      +------------------+----------------------+-------------+-----------------------+---------------------+
      |       NAME       |      FOLDER ID       |  MAX SIZE   | DEFAULT STORAGE CLASS |     CREATED AT      |
      +------------------+----------------------+-------------+-----------------------+---------------------+
      | first-bucket     | b1gmit33ngp6******** | 53687091200 | STANDARD              | 2022-12-16 13:58:18 |
      +------------------+----------------------+-------------+-----------------------+---------------------+
      ```

  1. Создайте составную загрузку:

      ```bash
      yc storage s3api create-multipart-upload \
        --bucket <имя_бакета> \
        --key <ключ_объекта>
      ```

      Где:

      * `--bucket` — имя вашего бакета.
      * `--key` — [ключ](../../concepts/object.md#key) объекта, по которому объект будет храниться в бакете.

      Результат:

      ```text
      request_id: 644e7c89********
      bucket: first-bucket
      key: video.mp4
      upload_id: 000629B7********
      ```

      Сохраните значение идентификатора составной загрузки (`upload_id`) — оно понадобится для выполнения следующих шагов.

      Подробнее о команде `yc storage s3api create-multipart-upload` читайте в [справочнике CLI](../../../cli/cli-ref/storage/cli-ref/s3api/create-multipart-upload.md).

- AWS CLI {#aws-cli}

  Если у вас еще нет AWS CLI, [установите и сконфигурируйте его](../../tools/aws-cli.md).

  1. Получите список доступных бакетов:

      ```bash
      aws s3api list-buckets \
        --endpoint-url=https://{{ s3-storage-host }} 
      ```

      Результат:

      ```json
      {
        "Buckets": [
          {
            "Name": "new-bucket",
            "CreationDate": "2024-12-20T17:48:17+00:00"
          },
          {
            "Name": "first-bucket",
            "CreationDate": "2023-12-28T12:20:49.538000+00:00"
          }
        ],
        "Owner": {
          "DisplayName": "",
          "ID": ""
        }
      }
      ```

  1. Создайте составную загрузку:

      ```bash
      aws s3api create-multipart-upload \
        --endpoint-url=https://{{ s3-storage-host }} \
        --bucket <имя_бакета> \
        --key <ключ_объекта>
      ```

      Где:

      * `--bucket` — имя вашего бакета.
      * `--key` — [ключ](../../concepts/object.md#key) объекта, по которому объект будет храниться в бакете.

      Результат:

      ```json
      {
        "Bucket": "first-bucket",
        "Key": "video.mp4",
        "UploadId": "000629C2********"
      }
      ```

      Сохраните значение идентификатора составной загрузки (`UploadId`) — оно понадобится для выполнения следующих шагов.

- API {#api}

  Чтобы создать составную загрузку объекта в бакете, воспользуйтесь методом S3 API [startUpload](../../s3/api-ref/multipart/startupload.md).

{% endlist %}

## Загрузите в бакет составные части объекта {#upload-parts}

Загрузите первую часть объекта:

{% list tabs group=instructions %}

- {{ yandex-cloud }} CLI {#cli}

  1. Посмотрите описание команды CLI для загрузки составной части объекта в бакет:

      ```bash
      yc storage s3api upload-part --help
      ```

  1. Выполните команду:

      ```bash
      yc storage s3api upload-part \
        --bucket <имя_бакета> \
        --key <ключ_объекта> \
        --upload-id <идентификатор_составной_загрузки> \
        --part-number <порядковый_номер_части> \
        --body <путь_к_файлу_с_составной_частью>
      ```

      Где:

      * `--bucket` — имя вашего бакета.
      * `--key` — [ключ](../../concepts/object.md#key) объекта, по которому объект будет храниться в бакете.
      * `--upload-id` — значение идентификатора составной загрузки, полученное на предыдущем шаге.
      * `--part-number` — порядковый номер составной части объекта, загружаемой в бакет.

          Порядковый номер однозначно идентифицирует часть и определяет ее порядок в общей последовательности. Номер — это целое число в промежутке от 1 до 10000 включительно.
          
          Если загружаются несколько частей с одинаковым номером, {{ objstorage-name }} сохраняет последнюю поступившую.
          
          Размер каждой части, кроме последней, должен быть не менее 5 МБ
      * `--body` — путь к файлу, в котором сохранена составная часть на компьютере пользователя.

      Результат:

      ```text
      request_id: 7ab2820f********
      etag: '"a2c44573954874e1a62a3f59********"'
      ```

      Сохраните значение `etag` — оно понадобится для сборки объекта из частей на стороне бакета {{ objstorage-name }}.

      Подробнее о команде `yc storage s3api upload-part` читайте в [справочнике CLI](../../../cli/cli-ref/storage/cli-ref/s3api/upload-part.md).

- AWS CLI {#aws-cli}

  ```bash
  aws s3api upload-part \
    --endpoint-url=https://{{ s3-storage-host }} \
    --bucket <имя_бакета> \
    --key <ключ_объекта> \
    --upload-id <идентификатор_составной_загрузки> \
    --part-number <порядковый_номер_части> \
    --body <путь_к_файлу_с_составной_частью>
  ```

  Где:

  * `--bucket` — имя вашего бакета.
  * `--key` — [ключ](../../concepts/object.md#key) объекта, по которому объект будет храниться в бакете.
  * `--upload-id` — значение идентификатора составной загрузки, полученное на предыдущем шаге.
  * `--part-number` — порядковый номер составной части объекта, загружаемой в бакет.

      Порядковый номер однозначно идентифицирует часть и определяет ее порядок в общей последовательности. Номер — это целое число в промежутке от 1 до 10000 включительно.
      
      Если загружаются несколько частей с одинаковым номером, {{ objstorage-name }} сохраняет последнюю поступившую.
      
      Размер каждой части, кроме последней, должен быть не менее 5 МБ
  * `--body` — путь к файлу, в котором сохранена составная часть на компьютере пользователя.

  Результат:

  ```json
  {
      "ETag": "\"a2c44573954874e1a62a3f59********\""
  }
  ```

  Сохраните значение `ETag` — оно понадобится для сборки объекта из частей на стороне бакета {{ objstorage-name }}.

- API {#api}

  Чтобы загрузить в бакет составную часть объекта, воспользуйтесь методом S3 API [uploadPart](../../s3/api-ref/multipart/uploadpart.md).

{% endlist %}

Аналогичным образом загрузите остальные составные части объекта, последовательно заменяя в командах порядковый номер `--part-number` и путь к файлу составной части `--body`. Не забывайте сохранять для каждой загруженной части полученное уникальное значение `etag`.

## Завершите составную загрузку {#complete-upload}

После успешной загрузки всех составных частей, завершите составную загрузку и соберите загруженные в бакет части в целый объект:

{% list tabs group=instructions %}

- {{ yandex-cloud }} CLI {#cli}

  1. Посмотрите описание команды CLI для завершения составной загрузки объекта в бакет:

      ```bash
      yc storage s3api complete-multipart-upload --help
      ```

  1. Выполните команду:

      ```bash
      yc storage s3api complete-multipart-upload \
        --bucket <имя_бакета> \
        --key <ключ_объекта> \
        --upload-id <идентификатор_составной_загрузки> \
        --multipart-upload \
          '{"Parts": [{"PartNumber": <порядковый_номер_части_1>, "ETag": "<значение_etag_части_1>"}, {"PartNumber": <порядковый_номер_части_2>, "ETag": "<значение_etag_части_2>"}, ..., {"PartNumber": <порядковый_номер_части_n>, "ETag": "<значение_etag_части_n>"}]}'
      ```

      Где:

      * `--bucket` — имя вашего бакета.
      * `--key` — [ключ](../../concepts/object.md#key) объекта, по которому объект будет храниться в бакете.
      * `--upload-id` — значение идентификатора составной загрузки, полученное на предыдущем шаге.
      * `--multipart-upload` — объект, содержащий порядковые номера и значения `etag` всех загруженных составных частей в нужном порядке. Например:

          ```json
          --multipart-upload \
            '{"Parts": [{"PartNumber": 1, "ETag": "a2c44573954874e1a62a3f59********"}, {"PartNumber": 2, "ETag": "3c0e70e1d05bd4e2500f5a90********"}, {"PartNumber": 3, "ETag": "953ecfff0150fd0970008320********"}]}'
          ```

      Результат:

      ```text
      request_id: bde94875********
      bucket: first-bucket
      key: video.mp4
      etag: '"040d1fe80bd1d4f12728b192********-3"'
      location: https://{{ s3-storage-host }}:443/first-bucket/video.mp4
      ```

      Подробнее о команде `yc storage s3api complete-multipart-upload` читайте в [справочнике CLI](../../../cli/cli-ref/storage/cli-ref/s3api/complete-multipart-upload.md).

- AWS CLI {#aws-cli}

  ```bash
  aws s3api complete-multipart-upload \
    --endpoint-url=https://{{ s3-storage-host }} \
    --bucket <имя_бакета> \
    --key <ключ_объекта> \
    --upload-id <идентификатор_составной_загрузки> \
    --multipart-upload \
      '{"Parts": [{"PartNumber": <порядковый_номер_части_1>, "ETag": "<значение_etag_части_1>"}, {"PartNumber": <порядковый_номер_части_2>, "ETag": "<значение_etag_части_2>"}, ..., {"PartNumber": <порядковый_номер_части_n>, "ETag": "<значение_etag_части_n>"}]}'
  ```

  Где:

  * `--bucket` — имя вашего бакета.
  * `--key` — [ключ](../../concepts/object.md#key) объекта, по которому объект будет храниться в бакете.
  * `--upload-id` — значение идентификатора составной загрузки, полученное на предыдущем шаге.
  * `--multipart-upload` — объект, содержащий порядковые номера и значения `etag` всех загруженных составных частей в нужном порядке. Например:

      ```json
      --multipart-upload \
        '{"Parts": [{"PartNumber": 1, "ETag": "a2c44573954874e1a62a3f59********"}, {"PartNumber": 2, "ETag": "3c0e70e1d05bd4e2500f5a90********"}, {"PartNumber": 3, "ETag": "953ecfff0150fd0970008320********"}]}'
      ```

  Результат:

  ```json
  {
    "Location": "https://{{ s3-storage-host }}/first-bucket/video.mp4",
    "Bucket": "first-bucket",
    "Key": "video.mp4",
    "ETag": "\"040d1fe80bd1d4f12728b192********-3\""
  }
  ```

- API {#api}

  Чтобы завершить составную загрузку объекта, воспользуйтесь методом S3 API [completeUpload](../../s3/api-ref/multipart/completeupload.md).

{% endlist %}

Объект с заданным префиксом создан в бакете {{ objstorage-name }} из загруженных составных частей. Воспользуйтесь [инструкцией](./info.md), чтобы получить информацию о созданном объекте.