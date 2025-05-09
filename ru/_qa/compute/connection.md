# Подключение

#### Как подключиться к ВМ с Linux {#conn-vm-linux}

Воспользуйтесь инструкцией: [{#T}](../../compute/operations/vm-connect/ssh.md). Если на компьютере, с которого вы подключаетесь, старая версия Windows (7, 8 и первые выпуски 10) и нет встроенного консольного [SSH-клиента](../../glossary/ssh-keygen.md), используйте [PuTTY](https://www.putty.org/).

В образах Linux, предоставляемых {{ yandex-cloud }}, изначально нет графической оболочки. Возможность подключения по [SSH](../../glossary/ssh-keygen.md) с использованием логина и пароля по умолчанию отключена, поскольку этот способ является устаревшим и небезопасным.

#### Что делать, если после создания ВМ ни разу не удалось подключиться? {#fail-connection}

Ваше устройство должно «видеть» ВМ по [сети](../../vpc/concepts/network.md#network):
* Если вы подключаетесь извне, за ВМ должен быть закреплен [публичный IP-адрес](../../vpc/concepts/address.md#public-addresses) или настроена связность через другую ВМ с публичным IP-адресом (например, через [NAT-инстанс](../../tutorials/routing/nat-instance/index.md)).
* На вашем устройстве должен быть разрешен исходящий трафик в сторону ВМ и входящий от нее.

Сразу после создания ВМ ее не следует выключать или перезагружать. Для ВМ с малой гарантированной долей vCPU процесс первого запуска может занять длительное время (до получаса). Если работа ВМ завершена преждевременно, скрипты инициализации могли не успеть выполниться. В этом случае удалите ВМ и создайте новую.

#### Как использовать root на ВМ Linux? {#use-root-linux}

При [создании ВМ с операционной системой Linux](../../compute/operations/vm-create/create-linux-vm.md) не используйте логины, зарезервированные системой, такие как `root`, `admin` и т. п., поскольку `cloud-init` не сможет добавить пользователя с таким именем. Попробуйте создать ВМ, указав логин пользователя `user`.

Для повышения прав до `root` используйте `sudo`. Пользователь, указанный при создании ВМ, входит в группу `sudo`. У `root` нет пароля, поэтому достаточно подключиться к ВМ по SSH (см. инструкцию [{#T}](../../compute/operations/vm-connect/ssh.md)) и ввести команду:

```bash
sudo su
```

Вы также можете выполнять различные команды, не переключаясь на `root`. Например, для выключения ВМ из гостевой ОС достаточно ввести команду:

```bash
sudo poweroff
```

#### Что делать, если не удается подключиться к ВМ, хотя ранее подключение работало? {#new-fail-connection}

Вероятно, вашими данными завладели, и к ВМ получили доступ третьи лица. Сделайте [снимок диска](../../compute/concepts/snapshot.md) ВМ (см. инструкцию [{#T}](../../compute/operations/disk-control/create-snapshot.md)) и создайте новую ВМ из него (см. [{#T}](../../compute/operations/vm-create/create-from-snapshots.md)). Если к новой ВМ удалось подключиться, используйте более сложные пароли и не распространяйте ваши секретные данные.

Если ВМ, созданная из снимка, также недоступна для подключения, воспользуйтесь рекомендациями [Как достать важные данные со сломанной ВМ](#get-data-from-fail-vm).

#### Что делать, если не удается подключиться к ВМ из образа {{ marketplace-name }}? {#fail-connection-marketplace}

В некоторых образах {{ marketplace-name }} зарезервированы служебные логины, например `bitrix` в образе [1С-Битрикс](/marketplace/products/yc/bitrix-1c-centos-7). Для создания и подключения к ВМ используйте логин, который не зарезервирован в системе, например `user`. Не используйте `root`, `admin` или другие системные логины.

В дальнейшем вы сможете сменить пароль для пользователя `bitrix` командой: `sudo passwd bitrix`.

#### Что делать, если возникает ошибка Permission denied? {#permission-denied}

При [подключении](../../compute/operations/vm-connect/ssh.md#vm-connect) к ВМ по SSH могут возникать подобные ошибки:

```bash
Permission denied (publickey)
Permission denied (publickey,gssapi-keyex,gssapi-with-mic)
```

Частые причины ошибок:

* При создании виртуальной машины указан открытый ключ с опечаткой.
* При попытке подключения передан неверный закрытый ключ.
* В файле `authorized_keys` указаны неверные данные.

Чтобы решить проблему, при подключении попробуйте указать нужный закрытый ключ с помощью флага `-i`:

  ```bash
  ssh -i <путь_к_ключу/имя_файла_ключа> <имя_пользователя>@<публичный_IP-адрес_ВМ>
  ```

#### Как настроить RDP/VNC-подключение к ВМ с Linux? {#set-rdp-vnc-to-linux}

Для подключения графического интерфейса на ВМ с ОС Linux есть два варианта:
* Использовать RDP или VNC. Для этого:
  1. Установите окружение рабочего стола на ВМ на базе ОС Linux (Ubuntu/CentOS).
  1. Установите xrdp или VNC-сервер для подключения по RDP или VNC соответственно.
  1. Настройте ПО.

     Рекомендуем использовать связку VNC-сервера, прослушивающего только localhost, и SSH-туннеля: в этом случае вы исключите риск подключения злоумышленников к вашей ВМ по VNC, а также зашифруете VNC-трафик. Возможно, вам будет полезна [статья на Хабре](https://habr.com/ru/company/ua-hosting/blog/273201/).
* Настроить X11 forwarding на локальный компьютер. Для Windows используйте [Xming](https://sourceforge.net/projects/xming/).
  1. Установите окружение рабочего стола.
  1. Настройте X11 forwarding и подключитесь по SSH.
  1. Запустите приложение из терминала.

#### Что делать, если не удается подключиться к ВМ, хотя ранее подключение работало? {#new-fail-connection}

Вероятно, вашими данными завладели, и к ВМ получили доступ третьи лица. Сделайте снимок диска ВМ (см. инструкцию [{#T}](../../compute/operations/disk-control/create-snapshot.md)) и создайте новую ВМ из него (см. [{#T}](../../compute/operations/vm-create/create-from-snapshots.md)). Если к новой ВМ удалось подключиться, используйте более сложные пароли и не распространяйте ваши секретные данные.

Если ВМ, созданная из снимка, также недоступна для подключения, воспользуйтесь рекомендациями [Как достать важные данные со сломанной ВМ](#get-data-from-fail-vm).

#### Как использовать серийную консоль? Как подключиться к ВМ напрямую? {#connection-linux-via-console}

При возникновении проблем с подключением к ВМ по сети вы можете использовать серийную консоль. Подробнее о ней в разделе [{#T}](../../compute/operations/serial-console/index.md).

Включить доступ к серийной консоли можно при создании или изменении ВМ. Серийная консоль будет доступна в [консоли управления]({{ link-console-main }}) {{ yandex-cloud }} в меню ВМ.

Для ВМ с Linux дополнительно необходимо заранее настроить парольный доступ для пользователя, подключившись по SSH и задав пользователю пароль командой `sudo passwd user` (вместо `user` следует указать имя пользователя, заданное при создании ВМ).

#### Что делать, если утерян SSH-ключ? {#lost-ssh}

{% include [new-connect-ssh](new-connect-ssh.md) %}

#### Почему после перезагрузки не работает ВМ? {#why-fail-after-reboot}

ВМ может перестать работать после перезагрузки по одной из следующих причин:
* ВМ была принудительно перезагружена во время записи на системный диск. В этом случае могла быть повреждена файловая система.
* Был неправильно настроен файрвол и/или сеть.
* Были внесены изменения в настройки `cloud-init`.
* Была изменена системная версия Python, которая используется в том числе `cloud-init`.
* Произошла неполадка в сервисе.

Если вы совершали одно из этих действий, воспользуйтесь рекомендациями [Как достать важные данные со сломанной ВМ](#get-data-from-fail-vm). В ином случае оставьте обращение.

#### Как достать важные данные со сломанной ВМ? {#get-data-from-fail-vm}

При выходе из строя ВМ:
1. Сделайте снимок проблемного диска: см. инструкцию [{#T}](../../compute/operations/disk-control/create-snapshot.md).
1. Создайте новую ВМ с дополнительным (не загрузочным) диском, восстановленным из снимка: см. [{#T}](../../compute/operations/vm-create/create-from-snapshots.md).
1. Подключитесь к ВМ: см. [{#T}](../../compute/operations/index.md#vm-use).
1. Смонтируйте диск: см. [{#T}](../../compute/operations/vm-control/vm-attach-disk.md#mount-disk-and-fix-uuid).
1. Проведите проверку файловой системы диска.
1. Перенесите необходимые данные на загрузочный диск новой ВМ.