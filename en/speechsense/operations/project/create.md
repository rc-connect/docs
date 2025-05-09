---
title: Creating a project in {{ speechsense-full-name }}
description: Follow this guide to create a project in {{ speechsense-name }}.
---

# Creating a project

To create a project, you need either the `{{ roles-speechsense-admin }}` or `{{ roles-speechsense-editor }}` role for the space.

{% note info %}

Before creating a project, make sure to [create a connection](../connection/create.md).

{% endnote %}

To create a project:

1. Open the {{ speechsense-name }} [home page]({{ link-speechsense-main }}).
1. Go to the space of your choice.
1. Click ![create](../../../_assets/console-icons/folder-plus.svg) **{{ ui-key.yc-ui-talkanalytics.projects.create-project }}**.
1. Name the project and, optionally, enter a description.
1. Under **{{ ui-key.yc-ui-talkanalytics.connections.connection }}**, click **{{ ui-key.yc-ui-talkanalytics.projects.add-connection }}** and select the required item from the list.
1. (Optional) Add rules for filtering dialogs based on the connection's metadata. Filtering is not supported for fields of the **Date** type.
1. Click **{{ ui-key.yc-ui-talkanalytics.projects.create-project }}**.
