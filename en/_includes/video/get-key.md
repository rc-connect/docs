{% list tabs group=instructions %}

- {{ video-name }} interface {#console}

  1. Open the {{ video-name }} [home page]({{ link-video-main }}).
  1. Select a channel.
  1. In the ![image](../../_assets/console-icons/antenna-signal.svg) **{{ ui-key.yacloud_video.streams.title_streams }}** tab, select the broadcast you need.
  1. In the stream settings window that opens, copy the following field values:

      * **{{ ui-key.yacloud_video.stream-lines.label_stream-key }}**
      * **{{ ui-key.yacloud_video.stream-lines.label_server-address }}**

- API {#api}

  Use the [get](../../video/api-ref/StreamLine/get.md) REST API method for the [StreamLine](../../video/api-ref/StreamLine/index.md) resource or the [StreamLineService/Get](../../video/api-ref/grpc/StreamLine/get.md) gRPC API call.

  Use the [getStreamKey](../../video/api-ref/StreamLine/getStreamKey.md) REST API method for the [StreamLine](../../video/api-ref/StreamLine/index.md) resource or the [StreamLineService/GetStreamKey](../../video/api-ref/grpc/StreamLine/getStreamKey.md) gRPC API call.

{% endlist %}
