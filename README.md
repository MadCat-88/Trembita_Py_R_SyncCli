# Синхронний REST-клієнт з підтримкою системи «Трембіта»

## Опис клієнту

REST-клієнт, описаний в даній інструкції, розроблений мовою програмування Python з використанням фреймворку FastAPI і є сумісним з системою «Трембіта».

FastAPI – це сучасний, швидкий (високопродуктивний) web-фреймворк для побудови API з Python 3.10+ на основі стандартних асинхронних викликів.

Даний клієнт передбачає отримання відомостей про інформаційні об’єкти (користувачів) та управління їх статусом, у тому числі відправку запитів на пошук, отримання, створення, редагування та видалення об’єктів до відповідного сервісу. 

Додатково передбачено функції завантаження архівів з підписаними електронними повідомленнями-запитами, повідомленнями-відповідями у вигляді ASiC-контейнерів, а також генерацію ключів і сертифікатів для взаємної автентифікації між REST-клієнтом та ШБО системи «Трембіта».

Для демонстрації інтеграції з системою «Трембіта» було розроблено [вебсервіс](https://github.com/MadCat-88/Trembita_Py_R_SyncSrv) для роботи з даним вебклієнтом.

## Вимоги до програмного забезпечення 
| ПЗ            |   Версія   | Примітка                                                                                                                                                                                               |
|:--------------|:----------:|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ubuntu Server |   24.04    | Рекомендовані характеристики віртуальної машини:<br/> CPU: 1 <br/> RAM: 512 Мб                                                                                                                         |
| Python        | **3.10!**  | За допомогою скрипта встановлюється автоматично.<br/> Також можна встановити вручну при виборі відповідного типу інсталяції.<br/>**Важливо!** Якщо версія Python нижче 3.10, клієнт працювати не буде. |
| Git           |            | Для клонування репозиторію                                                                                                                                                                             |

## Залежності

Залежності програмного забезпечення клієнту зазначені в файлі `requirements.txt`.
  
## Структура проєкту

Проєкт складається з наступних файлів та директорій:

```
web-client_trembita_sync/
├── Dockerfile                    # Докер файл для контейнерезації застосунку   
├── LICENSE                       # Ліцензія
├── README.Docker.md              # Документація
├── README.md                     # Документація
├── app.py                        # Точка входу застосунку
├── asic                          # Папка для збереженя асік контейнерів з результатами обмінів
├── certs                         # Папка для ключа та сертифіката додатку для https, та для сертифікату ШБО 
│    ├── cert.pem       
│    └── key.pem
├── compose.yaml                    
├── config.ini                    # Конфігураційний файл застосунку
├── deploy.sh                     # Скрипт автоматичного встановлення
├── docs                          # Документація 
│    ├── docker_installation.md   # Документація
│    ├── manual_installation.md   # Документація
│    └── script_installation.md   # Документація
├── remove.sh                     # Скрипт автоматичного видалення
├── requirements.txt              # Залежності застосунку
├── templates                     # Папка з шаблонами веб сторінок застосунку 
│    ├── create_person.html       # Шаблон веб сторінки
│    ├── error.html               # Шаблон веб сторінки
│    ├── list_certs.html          # Шаблон веб сторінки
│    ├── list_files.html          # Шаблон веб сторінки
│    ├── list_person.html         # Шаблон веб сторінки
│    ├── navbar.html              # Шаблон веб сторінки
│    ├── search_form.html         # Шаблон веб сторінки
│    └── user_form.html           # Шаблон веб сторінки
└── utils.py                      # Бібліотека взаемодії з Трембітою
```

## Інсталяція клієнту

Клієнт можна інсталювати за допомогою скрипта автоматичного встановлення або вручну. Також клієнт може працювати в Docker.

- [Інсталяція клієнту за допомогою скрипта автоматичного встановлення](./docs/script_installation.md).
- [Інсталяція клієнту вручну](./docs/manual_installation.md).
- [Конфігурація клієнту](./docs/configuration.md).
- [Pозгортання вебклієнту в Docker ](./docs/docker_installation.md).

## Адміністрування клієнту

### Запуск вебклієнту

Для запуска вебклієнту необхідно виконати наступну команду:

```bash
sudo systemctl start fastapi_trembita_client
```

### Доступ до вебклієнту

Для доступу до вебінтерфейсу вебклієнту необхідно перейти за посиланням: http://<your_server_ip>:5000


### Перевірка статусу вебклієнту

Для того, щоб перевірити статус вебклієнту необхідно виконати наступну команду:

```bash
sudo systemctl status fastapi_trembita_client
```

### Зупинка вебклієнту

Для зупинки роботи вебсервісу необхідно виконати наступну команду:
```bash
sudo systemctl stop fastapi_trembita_client
```

### Видалення вебклієнту

Для видалення вебклієнту та всіх пов'язаних компонентів було створено скрипт `remove.sh`.

Даний скрипт після запуску, зупинить та видалить вебклієнт, видалить віртуальне середовище, клонований репозиторій та системні залежності.

Для того, щоб запустити скрипт необхідно:

1. Зробити файл виконуваним:

```bash
chmod +x remove.sh
```

2. Запустити скрипт:

```bash
./remove.sh
```

### Перегляд журналу подій

За замовчуванням журнал подій зберігається у файлі `fastapi_trembita_client.log`.

Конфігурація параметрів журналювання подій виконується в файлі `config.ini`. Детальніше з параметрами журналювання в даному файлі можна ознайомитися в настановах з [конфігурування](./docs/configuration.md).

Для того, щоб переглянути журнал подій вебклієнту необхідно виконати команду:

```bash
journalctl -u fastapi_trembita_client -f
```

### Налаштування HTTPS

Для налаштування з’єднання з ШБО системи «Трембіта» за допомогою протоколу HTTPS необхідно виконати наступні кроки:

1. Вказати відповідний протокол в файлі конфігурації (файл `config.ini`, детальніше див.[настанови з конфігурування](./docs/configuration.md)).

2. Завантажити сертифікат вебклієнта (вкладка «Сертифікати» в вебінтерфейсі вебклієнта, файл `cert.pem`) до відповідної підсистеми ШБО.

3. Завантажити внутрішній TLS сертифікат ШБО до директорії сертифікатів вебклієнту ( за замовчуванням /web-client_trembita_sync/certs/, детальніше див.[настанови з конфігурування](./docs/configuration.md)).

**Важливо:** Веблкієнт підтримує лише сертифікати формату PEM

## Інтеграція вебклієнту з системою «Трембіта»

Для повноцінної інтеграції з системою «Трембіта» вебклієнт повинен мати можливість додавати до запиту HTTP заголовки, необхідні для передачі запитів між шлюзами безпечного обміну системи «Трембіта». Окрім цього для ідентифікації повідомлень в власній інформаційній системі вебклієнт для кожного запиту повинен задавати унікальний ідентифікатор з заголовком Uxp-queryID. Більш детальну інформацію про заголовки наведено в [описі роботи із REST-сервісами в системі «Трембіта»](https://github.com/MadCat-88/Services-development-for-Trembita-system/blob/main/REST%20services%20development%20for%20Trembita%20system.md#%D0%B7%D0%B0%D0%B3%D0%BE%D0%BB%D0%BE%D0%B2%D0%BA%D0%B8-%D0%B7%D0%B0%D0%BF%D0%B8%D1%82%D1%96%D0%B2-%D0%B4%D0%BB%D1%8F-rest-%D1%81%D0%B5%D1%80%D0%B2%D1%96%D1%81%D1%96%D0%B2-%D0%BD%D0%B5%D0%BE%D0%B1%D1%85%D1%96%D0%B4%D0%BD%D1%96-%D0%B7%D0%B0%D0%B4%D0%BB%D1%8F-%D0%B7%D0%B0%D0%B1%D0%B5%D0%B7%D0%BF%D0%B5%D1%87%D0%B5%D0%BD%D0%BD%D1%8F-%D1%81%D1%83%D0%BC%D1%96%D1%81%D0%BD%D0%BE%D1%81%D1%82%D1%96-%D0%B7-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%BE%D1%8E-%D1%82%D1%80%D0%B5%D0%BC%D0%B1%D1%96%D1%82%D0%B0).

В даному вебклієнті конфігурування заголовків здійснюється в файлі конфігурації `config.ini`. Детальніше конфігурування заголовків описано в [настановах з конфігурування](./docs/configuration.md)

## Використання вебклієнту

Для доступу до вебінтерфейсу розробленого вебклієнту необхідно перейти за посиланням:

```
http://<your_server_ip>:5000
```

В даному інтерфейсі можна виконати наступні дії:
1.	Здійснювати пошук записів в БД за певними параметрами.
2.	Створювати записи в БД.
3.	Модифікувати записи в БД 

**Важливо!** Обмеження сервісу – не можна міняти поле unzr!

4.	Видаляти записи в БД за УНЗР.
5.	В разі, якщо для зв’язку з ШБО системи «Трембіта» використовується HTTPS, буде доступна можливість завантажувати ASIC-контейнери з запитами від вебклієнта до вебсервісу, які були передані через даний ШБО.
6.	Завантажити сертифікат даного вебклієнту для взаємної автентифікації з ШБО. Дану дію можна виконати на вкладці «Сертифікати».


## Внесок

Якщо ви хочете зробити свій внесок у проєкт, будь ласка, створіть форк репозиторію і відправте Pull Request.

## Ліцензія

Цей проект ліцензується відповідно до умов MIT License.

##
Матеріали створено за підтримки проєкту міжнародної технічної допомоги «Підтримка ЄС цифрової трансформації України (DT4UA)».
