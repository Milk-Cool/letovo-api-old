<!--- ![UNDER CONSTRUCTION](https://media.istockphoto.com/vectors/under-construction-site-banner-sign-vector-black-and-yellow-diagonal-vector-id1192837450?k=20&m=1192837450&s=612x612&w=0&h=MpHAjpQ7v_zZmH_2FjcbmVMonTOkjs156B1egVrFViw=) -->

# letovo-api
A document describing the student.letovo.ru API (RUSSIAN)<br>
Документ, расписывающий API student.letovo.ru

# PHPSESSID
Чтобы получить PHPSESSID, достаточно сделать GET-запрос на `https://student.letovo.ru` и из header'а `Set-Cookie` достать значение куки `PHPSESSID`. Это идентификатор сессии, который позволяет серверу понять, кто есть кто - его **нужно отправлять с каждым запросом**.
> Если забыть про PHPSESSID, то сервер просто не распознаст сессию и не поймёт, что это вы. Любой запрос без PHPSESSID, по сути, ничего не делает. *Поэтому важно его всегда указывать.*

# Вход
Для того, чтобы войти, надо сделать POST-запрос на `https://student.letovo.ru/preferences_login.php`.
### Тело запроса
URL-закодированные параметры:
- `act`: `logg`
- `key`: `1`
- `login`: летовский логин (пример: `2023ivanov.av`). Можно и с "хвостиком" (т. е. с `@student.letovo.ru` на конце).
- `pass`: летовский пароль (пример: `Diz25147`)

# OTP
После входа понадобится ввести одноразовый пароль, который приходит на почту при попытке входа. После ввода открывается доступ к личному кабинету.<br>
Для получения одноразового пароля на почту нужно сделать GET-запрос на `https://student.letovo.ru/index.php` со своим PHPSESSID.
> Одноразовый пароль не бесполезен - хоть по умолчанию пароль от личного кабинета и почты одинаковый, пароль от почты можно поменять, чтобы вас уж точно не взломали - я даже знаю нескольких людей, которые так и делают.

OTP нужно послать через POST-запрос на `https://student.letovo.ru/index.php`.
### Тело запроса
URL-закодированные параметры:
- `act`: `check_otp`
- `otp`: сам OTP, состоит из шести цифр

# А теперь самое интересное - что с этим всем теперь можно делать!
## Содержание
- [Локеры](https://github.com/Milk-Cool/letovo-api/blob/main/README.md#%D0%BB%D0%BE%D0%BA%D0%B5%D1%80%D1%8B)
- [Экзиаты](https://github.com/Milk-Cool/letovo-api/blob/main/README.md#%D1%8D%D0%BA%D0%B7%D0%B8%D0%B0%D1%82%D1%8B---%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D0%B8%D0%BB%D0%B8-%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5)
- [Удаление экзиатов](https://github.com/Milk-Cool/letovo-api/blob/main/README.md#%D1%8D%D0%BA%D0%B7%D0%B8%D0%B0%D1%82%D1%8B---%D1%83%D0%B4%D0%B0%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5)
- [Добавление достижений](https://github.com/Milk-Cool/letovo-api/blob/main/README.md#%D0%B4%D0%BE%D0%B1%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D0%B0)
- [Изменение достижений](https://github.com/Milk-Cool/letovo-api/blob/main/README.md#%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D0%B0)
- [Удаление достижений](https://github.com/Milk-Cool/letovo-api/blob/main/README.md#%D1%83%D0%B4%D0%B0%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D0%B0)
- [Получение расписания](https://github.com/Milk-Cool/letovo-api/blob/main/README.md#%D1%80%D0%B0%D1%81%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BA%D0%B0%D0%BA-ics-%D1%84%D0%B0%D0%B9%D0%BB)

# Локеры
Для изменения номеров локеров надо отправить POST-запрос на `https://student.letovo.ru/modules/student/student_ajax.php`.
### Тело запроса
URL-закодированные параметры:
- `act`: `change_locker`
- `id`: `1` - в академе, `2` - в гардеробе, `3` - в Arts
- `value` - номер локера

# Экзиаты - создание или изменение
Для создания или изменения уже существующих экзиатов надо отправить POST-запрос на `https://student.letovo.ru/modules/student/student_ajax.php`.
> К сожалению, номер только что созданного экзиата никак нельзя получить, кроме как парсить страницу `https://student.letovo.ru/index.php?part_student=exeat`. Вот так :/
### Тело запроса
URL-закодированные параметры:
- `act`: `save_exeat`
- `exeat_id`: ID (номер) экзиата (если экзиат создаётся в первый раз, то указывать не надо)
- `dttm_from`: Дата/время выхода из школы в формате `DD.MM.YYYY HH:MM:SS` / `ДД.ММ.ГГГГ ЧЧ.ММ.СС`
- `dttm_till`: Дата/время возвращения в том же формате
- `type`: 
- - `holiday/vacations` для "Праздник/каникулы"
- - `overnight` для "Выезд с ночёвкой"
- - `daily` для "Выезд с возвратом в тот же день"
- - `school_organized` для "Выезд организованный школой"
- - `regular` для "Регулярная поездка на выходные"
- - `medical` для "Медицинские причины"
- `transport`:
- - `parents_vehicle` для "Личный транспорт"
- - `schoolbus` для "Школьный автобус"
- - `public_city_transport` для "Общественный городской транспорт"
- - `taxi` для "Такси"
- - `plane_train` для "Самолёт/поезд"
- - `walk` для "Пешком"
- `who_escorts`:
- - `alone` для "Без сопровождения"
- - `my_parents` для "Родители/представители ученика"
- - `someones_parents` для "Родители другого ученика"
- - `custom_escort` для "Сопровождение другим лицом"
- `escorted_with_student`: Ученик/родители ученика, которые будут тебя сопровождать (необязательно; если нет, оставьте пустым)
- `escorted_by`: Сопровождающий (необязательно; если нет, оставьте пустым)
- `destination_type`: Не используется, но почему-то передаётся. Оставьте пустым.
- `destination_city`: Город назначения
- `destination_street`: Улица назначения
- `destination_building`: Номер дома назначения
- `reason`: Причина экзиата
- `alternative_mobile`: Телефон ученика
- `emergency_contact_name`: Имя сопровождающего (необязательно; если нет, оставьте пустым)
- `emergency_contact_number`: Номер мопровождающего (необязательно; если нет, оставьте пустым)

# Экзиаты - удаление
Чтобы удалить экзиат, надо послать POST-запрос на `https://student.letovo.ru/modules/student/student_ajax.php`.
> По особым причинам, экзиаты пока что не удаляются - [см. проблему на helpdesk](https://helpdesk.letovo.ru/WorkOrder.do?woMode=viewWO&woID=12215).
### Тело запроса
URL-закодированные параметры:
- `act`: `remove_exeat`
- `exeat_id`: ID (номер) экзиата

# Добавление результата
Чтобы добавить результат, нужно отправить POST-запрос на `https://student.letovo.ru/index.php`.
### Тело запроса
URL-закодированные параметры:
- `add_date`: дата события (формат `DD.MM.YYYY` / `ДД.ММ.ГГГГ`)
- `add_event`: название события
- `add_description`: краткое описание события

# Изменение результата
Чтобы изменить результат, нужно сделать такой же запрос, как выше, но также добавить параметр `id_add` - ID события.
> Если кто-то знает, как получить ID - дайте мне знать, а то я сам не знаю)))

# Удаление результата
Чтобы удалить результат, нужно сделать такой же запрос, как выше, но также добавить параметры `id_add` - ID события и `del_result` - можно и пустой, главное, чтобы был.

# Расписание как ICS-файл
Со своим PHPSESSID сделайте запрос на `https://student.letovo.ru/index.php?ics`. Ответом сервера как раз и будет расписание в формате ICS.

# ⠀
# Планы
Я планирую создать сервис, который восполнит все "дыры" здесь - например, получение списка экзиатов возможно только через HTML-страницу, как указано выше. Он будет получать страничку и парсить её, используя данный PHPSESSID. Но, наверное, я подожду нового учебного года, чтобы это всё можно было нормально протестировать. Удачи всем! ;)
