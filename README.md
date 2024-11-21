# ArchitectureTask

## Задача 1
Спроектировать многомодульное приложение "Чат". Есть 6 фичей - список юзеров, чат с юзером, профиль юзера, настройки, авторизация, регистрация. Кеширование отсутствует. Тип взаимодействия с сервером - REST. Сделать в виде схемы модулей, которые будут использоваться в проекте (архитектуру внутри модуля расписывать не надо).

### Стек: 
- Архитектура: 
	- Многомодульность
	- Clean Architecture + MVVM
 - UI: Compose
 - Сеть: 
	- Retrofit
- Навигация: Compose Navigation
- DI: Dagger2
- Асинхронность: Coroutine
- Уведомления: Firebase Cloud Messaging

### Modules

#### app
	- di - Dagger2
	- Связь модулей
#### core
	Общие инструменты
#### navigation
	Compose Navigation
#### network
	Retrofit
#### notifications
	обработка FCM сообщений
#### list_users
#### chat
#### profile
#### settings
#### authorization
#### registration
