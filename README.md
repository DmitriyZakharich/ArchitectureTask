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
		
Data
	models
		MessageDTO(id, text, time...)

	ChatRepositoryImpl
		getMessages(): Flow<List<Message>>
		{Взаимодействие с network модулем}
		
	mappers
		MessageDTO -> Message
```


## Задача 3 

### MVI
#### list_users
```
Presentation
	
	View
		viewModel.handleIntent(FetchList())
		viewModel.handleIntent(Navigate(chatID))

		fun observeViewModel()
			when (state){}

	ViewModel
		StateFlow<ListState>()
		fun handleIntent(intent: ListIntent)
		
	intents
		sealed class ListIntent{
			FetchList : ListIntent
			Navigate(chatID) : ListIntent
		}
			
	viewstate
		sealed class ListState{
		    data object Idle : ListState()
		    data object Loading : ListState()
		    data class Users(val data: List<User>) : ListState()
		    data class Error(val error: String?) : ListState()
    }
			
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
		
	mappers
		UserDTO -> User
```

#### chat
```
Presentation
	View
		viewModel.handleIntent(FetchMessages())
		viewModel.handleIntent(SendMessage(message))
		viewModel.handleIntent(BackPress())
		
		fun observeViewModel()
			when (state){}
		
	ViewModel
		StateFlow<ChatState>
		fun handleIntent(intent: ChatIntent)
		
	intents
		sealed class ChatIntent{
			FetchMessages : ChatIntent
			SendMessage(message) : ChatIntent
			BackPress() : ChatIntent
		}
		
	viewstate
		sealed class ChatState{
		    data object Idle : ChatState()
		    data object Loading : ChatState()
		    data class Messages(val data: List<Messages>) : ChatState(
		    data class Error(val error: String?) : ChatState()
    }
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


## MVP
#### list_users
```
Presentation

	interface ListContract{
		interface View
		interface Presenter
	}
	
	ListView : ListContract.View
		presenter.fetchList()

	ListPresenter : ListContract.Presenter
		view.showList(list)
			
Domain
	interface 
		UsersRepository
			getUsers(): List<User>

	models 
		User(id, name, avatarUrl...)

	use_cases
		getUsers(): Flow<List<User>>
		...
		
Data
	models
		UserDTO(id, name, avatarUrl...)

	UsersRepositoryImpl
		getUsers(): List<User>
		{Взаимодействие с network модулем}
		
	mappers
		UserDTO -> User
```

#### chat
```
Presentation
	interface ChatContract{
		interface View
		interface Presenter
	}

	ChatView : ChatContract.View
		presenter.fetchList()
		presenter.sendMessage(message)
		
	Presenter : ChatContract.Presenter
		view.showMessages(messages)
    
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
