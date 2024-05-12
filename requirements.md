## Идея
Пользователь легко может создавать пасты, делиться и управлять ими. Паста - кусочек кода или текстовая заметка.

## Нефункциональные требования к платформе
- интуитивно понятный интерфейс с подсказками

## Функциональные требования к платформе
### Для незарегистрированного пользователя
- **Работа с пастой**:
  - создание публичной пасты с получением ссылки для того, чтобы ей поделиться
  - удаление пасты с использованием секретного слова, указанного при создании пасты
  - редактирование пасты с использованием секретного слова, указанного при создании пасты
  - просмотр пасты по ссылке
  - просмотр последних добавленных публичных паст
  - просмотр профиля пользователя
  - получение списка публичных паст пользователя:
    - с использованием фильтров (тип разметки, теги, публичность)
    - с использованием поиска по названию и контенту
    - с использованием сортировки по дате создания, количеству лайков
- **Регистрация и вход**:
  - с использованием GitHub-аккаунта
  - с использованием логина, пароля и почты (на которую будет оптравлено письмо с подтверждением регистрации)
  - сброс пароля при указании почты

### Для авторизированного пользователя
- **Работа с собственной пастой**:
  - создание, редактирование, удаление пасты
  - получение списка публичных и приватных паст
- **Работа с чужой пастой**:
  - добавление, снятие лайка с чужой пасты
  - добавление, удаление комментария
- **Работа с собственными тегами**:
  - создание, удаление, редактирование тега
  - просмотр списка тегов
  - добавление, удаление тега к пасте
- **Работа с профилем**:
  - изменение отображаемого имени пользователя и личной информации
  - изменение изображения аватара
- **Работа с аккаунтом**:
  - изменение пароля
  - изменение почты
  - удаление аккаунта
- **Выход из аккаунта**