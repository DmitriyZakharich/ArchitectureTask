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



## Задача 2
Cпроектировать схему взаимодействия между слоями clean architecture в рамках одного модуля для приложения "Чат". Будет только две фичи - список юзеров и чат с юзером. Внутри data-domain-presentaion слоев показать классы, которые будут использоваться. Использовать MVVM.


#### list_users
```
Presentation
	Compose + ViewModel
		getUser()
		goToChat(id)
Domain
	interface 
		UsersRepository
			getUsers(): Flow<List<User>>
	models 
		User(id, name, avatarUrl...)
	use_cases
		getUsers(): Flow<List<User>>
		...
Data
	models
		UserDTO(id, name, avatarUrl...)
	UsersRepositoryImpl
		getUsers(): Flow<List<User>>
		{Взаимодействие с network модулем}
		...
	mappers
		UserDTO -> User
```

#### chat
```
Presentation
	Compose + ViewModel
		sendMessage(message: Message)
		backPress()
Domain
	interface 
		ChatRepository
			getMessages(): Flow<List<Message>>
	models 
		Message(id, text, time...)
	use_cases
		getMessages(): Flow<List<Message>>
		sendMessage(message: Message)
		...
Data
	models
		MessageDTO(id, text, time...)
	ChatRepositoryImpl
		getMessages(): Flow<List<Message>>
		{Взаимодействие с network модулем}
		...
	mappers
		MessageDTO -> Message
```
