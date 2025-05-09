---
title: Отчет «Детализация» в {{ speechsense-name }}
---

# Отчет «Детализация» в {{ speechsense-name }}

В отчете **{{ ui-key.yc-ui-talkanalytics.reports.details-key-value }}** видно, как меняются значения параметра оценки во времени для каждого среза данных. Например, можно построить отчет по средней длительности диалогов, сгруппировать ее значения по подразделениям и указать период длительностью один год. В результате отчет покажет, как менялась средняя длительность диалогов в течение года для каждого подразделения.

## Как формируется отчет {#form}

Отчет **{{ ui-key.yc-ui-talkanalytics.reports.details-key-value }}** создается на основе настроек:

* [параметр оценки](#parameter) — изменение его значений отображается в отчете;
* [фильтры](#filters) — применяются к диалогам в отчете.

После того как вы задали настройки, можно [построить отчет](../../operations/data/manage-reports.md#build-a-details-report). Он показывает значения параметра оценки по нескольким срезам данных в виде [графика и таблицы](#display). Пример:

![image](../../../_assets/speechsense/details_report.jpeg)


### Параметр оценки {#parameter}

_Параметр оценки_ — это параметр, изменение значений которого можно посмотреть в отчете. Учитываются только численные параметры. Если выбрать [тег](../tags.md), в качестве его значения используется число раз, когда тег был назначен на диалоги.

В отчете считается суммарное, среднее, минимальное или максимальное значение параметра оценки за выбранный период. Например, такие значения могут быть полезны в следующих случаях:

* сумма — сколько всего раз клиент просил вызвать начальство во время разговора;
* среднее — средняя продолжительность диалога в секундах;
* минимум или максимум — у кого из операторов меньше или больше всего нарушений.

В отчете можно выбрать параметр оценки одного из следующих типов:

{% include [description-of-parameters](../../../_includes/speechsense/reports/parameters.md) %}

### Фильтрация в отчете {#filters}

{% include notitle [filters](../../../_includes/speechsense/reports/filters.md) %}

## Отображение данных в отчете и взаимодействие с ними {#display}

Отчет показывает количественные характеристики работы операторов. Он доступен в веб-интерфейсе {{ speechsense-name }} в виде графика и таблицы, также отчет можно скачать в формате CSV.

Форматы, в которых доступен отчет **{{ ui-key.yc-ui-talkanalytics.reports.details-key-value }}**:

* **{{ ui-key.yc-ui-talkanalytics.reports.barchart }}** — позволяет визуально оценить, какие значения принимает параметр оценки в разные моменты времени. Помимо периода, за который строится отчет, на графике также настраивается масштаб детализации данных. В результате можно получить значения за разные интервалы: от часа до квартала.
* **{{ ui-key.yc-ui-talkanalytics.reports.table-key-value }}** — показывает численное значение параметра оценки за указанный период. Значения разбиты в соответствии с настроенной группировкой данных.
* **CSV-файл** — содержит такую же таблицу, как в веб-интерфейсе {{ speechsense-name }}. Используйте формат CSV, чтобы сохранить отчет локально.
