#### Квоты {#search-api-quotas}

| Вид ограничения | Значение |
| ----- | ----- |
| **[API v1](../search-api/concepts/index.md#api-v1)** | | 
| Количество синхронных запросов в сутки | 100 000</br></br>Суточный лимит обновляется в 00:00:00 [GMT](https://ru.wikipedia.org/wiki/Среднее_время_по_Гринвичу) (00:03:00 [UTC+3](https://ru.wikipedia.org/wiki/UTC%2B3:00)) |
| Количество синхронных запросов в час (`hourly_limits`) | Зависит от времени суток^1^</br>С 00:00:00 до 00:59:59 — 30% от суточной квоты</br>С 00:59:59 до 03:59:59 — 40% от суточной квоты</br>С 04:00:00 до 07:59:59 — 60% от суточной квоты</br>С 08:00:00 до 08:59:59 — 40% от суточной квоты</br> С 09:00:00 до 09:59:59 — 30% от суточной квоты</br> С 10:00:00 до 10:59:59 — 20% от суточной квоты</br> С 11:00:00 до 22:59:59 — 10% от суточной квоты</br> С 23:00:00 до 23:59:59 — 20% от суточной квоты  |
| Количество синхронных запросов в секунду (`rps_restriction`) | Зависит от количества запросов в час:  `rps_restriction` = `hourly_limits`/3420  |
| **[API v2](../search-api/concepts/index.md#api-v2)** | |
| Количество синхронных запросов в час | 10 000 |
| Количество синхронных запросов в секунду | 10 |
| Количество синхронных запросов в секунду для получения генеративного ответа | 1 |
| Количество отложенных запросов в час | 35 000 |
| Количество отложенных запросов в секунду | 10 |
| Количество запросов в секунду на получение результатов отложенного запроса | 10 |

^1^ Время указано в часовом поясе [UTC+3](https://ru.wikipedia.org/wiki/UTC%2B3:00).

> Например, для квоты 100 000 запросов в сутки вы можете отправить не более 10 000 запросов каждый час в период с 11:00:00 до 22:59:59 (примерно 3 запроса в секунду) и не более 40 000 запросов (примерно 11 запросов в секунду) — с 08:00:00 до 08:59:59. 
> Если же вы поднимите квоту на количество запросов в сутки до 200 000, то сможете отправить не более 20 000 запросов каждый час в период с 11:00:00 до 22:59:59 (примерно 6 запросов в секунду) и не более 80 000 запросов (примерно 22 запроса в секунду) — с 08:00:00 до 08:59:59. 


#### Лимиты {#search-api-limits}

| Вид ограничения | Значение |
| ----- | ----- |
| Количество возвращаемых результатов | Не более 250 |
| Максимальная длина запроса | {{ search-api-request-ch }} |
| Максимальное количество слов в запросе | {{ search-api-request-w }} |
| Минимальное время обработки запроса в отложенном режиме | 5 минут |