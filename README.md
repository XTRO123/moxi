# MOXI - инструмент для быстрой настройки Modx Revo после установки

![moxii-logo](https://github.com/alexsoin/moxi/assets/3787132/701f2057-bfd2-44ee-b789-6e2551e68ca3)

## Быстрый старт

> !!! ВНИМАНИЕ !!!
> Запускать настройку можно исключительно на свежеустановленную систему! Запуская данный инструмент на уже рабочем сайте НЕЛЬЗЯ!

Подключаемся через `ssh`, переходим в корень сайта и клонируем [репозиторий](https://github.com/alexsoin/moxi)

```bash
git clone git@github.com:alexsoin/moxi.git
```

### WEB

Открываем `http://домен_сайта/moxi/` видим интерфейс настройки.

Можем изменить название сайта, поменять расположение панели управление, тоесть введя `panel` заместо `http://домен_сайта/manager/` в панель управления можно будет попасть по адресу `http://домен_сайта/panel/`. Подробнее про то зачем изменять адрес панели управления можете почитать [тут](https://zencod.ru/articles/harden-modx-revo). Также можно не заполнять эти поля и тогда название сайта и его адрес не изменятся.

Отдельно код скрипта смены адреса панели управления можете найти [тут](https://zencod.ru/gists/modx-rename-manager/).

Далее список дополнений. Галками отмечаем, какие дополнения нужно установить на сайт. Их список настраивается в файле `src/data/addons.php`. Далее ставим галку, чтобы удалить инструмент `moxi` после окончания настройки.

Самый времязатратный процесс настройки это установка дополнений, если на сервере стоят ограничения времени выполнения запроса, то все дополнения могут не успеть установиться, но не беда, запускаем `moxi` ещё раз и недостающие дополнения установятся. Исключение это скачанные, но не успевшие установиться дополнения, чтобы их установить нужно будет вручную через менеджер дополнений нажать `установить`, но если вы ставите не на тестовой какой-то среде, где время выполнения скрипта 30 секунд, то скорее всего всё успеет установиться.

По окончанию установки видим лог выполнения и над ним кнопки открывающие модальные окна с ошибками и предупреждениями.

На этом настройка завершена.

### CLI

Альтернативный способ запуска `moxi`. В отличие от web интерфейса нельзя изменить список устанавливаемых дополнений. Преимущество запуска через cli в том, что тут уже не будет ограничения на времени выполнения скрипта.

В томже терминале `ssh`, в котором склонировали репозиторий, переходим в директорию `moxi`.

```bash
cd moxi
```

И запускаем через php версии 7.4 консольную утилиту:

```bash
php7.4 ./cli.php
```

> На разных хостингах запуск php необходимой версии происходит по-разному, где-то `php7.4`, где-то `php74`, где-то `/usr/bin/php74/bin/php`. Для того чтобы узнать как на вашем хостинге запустить php нужной версии - читайте документацию, либо обращайтест в техподдержку хостинга.

Вводим логин и пароль администратора панели управления cms modx.

Далее указываем название сайта(если нужно его сменить), либо нажимаем сразу `enter` и тогда название не изменится.

Далее изменение названия панели управления, тут аналогично.

На следующем шаге отобразится список запускаемых процессов,  соглашаемся, вводим `Y` либо сразу нажимаем `enter` и начнётся настройка.

> Может кто не знает, в командной строке когда показывается окно подтверждения процесса запуска `[Y/n]` большая буква означает то, что применится по умолчанию без ввода символов, тоесть если `[y/N]` значит применится `n`.

Настройка завершена.

## Структура

Приложение имеет следующую структуру:

```bash
├── app.php                            // Главный класс
├── cli.php                            // Класс для работы в командной строке
├── web.php                            // Класс для работы через web интерфейс
├── index.html                         // UI
├── _frontend/                         // Исходники UI компонента
└── src/                               // Исходные данные проекта
    ├── content/                       // Контент
    │   ├── core/                      // Файлы директории core которые будут скопированы на сайт
    │   │   ├── components/
    │   │   │   └── translit/          // Фикс компонента translit
    │   │   └── elements/
    │   │       ├── zoomx/             // Файлы zoomx
    │   │       │   ├── controllers/   // Контроллеры zoomx (App\Controllers)
    │   │       │   ├── plugins/       // Плагины zoomx
    │   │       │   ├── snippets/      // Сниппеты zoomx
    │   │       │   └── templates/     // Шаблоны zoomx
    │   │       ├── chunks/            // Чанки fenom
    │   │       └── templates/         // Шаблоны fenom
    │   │           └── layouts/       // Макеты шаблонов fenom
    │   ├── pages/                     // Контент ресурсов
    │   ├── plugins/                   // Контент плагинов
    │   ├── snippets/                  // Контент сниппетов
    │   └── templates/                 // Контент шаблонов
    └── data/                          // Импортируемые данные
        ├── addons.php                 // Список пакетов разделенных по провайдерам
        ├── clientConfig.php           // Поля и группы полей для пакета ClientConfig
        ├── plugins.php                // Список плагинов и их настроек
        ├── providers.php              // Список провайдеров пакетов
        ├── resources.php              // Список ресурсов
        ├── settings.php               // Список системных настроек и их значений
        ├── snippets.php               // Список сниппетов
        ├── templates.php              // Список шаблонов
        └── tvs.php                    // Список тв параметров
```

## Ссылки

- [О инструменте](https://zencod.ru/articles/moxi/)
- [Кастомизация](https://zencod.ru/articles/moxi-settings/)
