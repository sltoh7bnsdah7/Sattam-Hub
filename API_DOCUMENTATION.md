# API Documentation - Sattam Hub

## Overview

This document describes the repository layer APIs used in the Sattam Hub application. All repositories follow a consistent pattern using Kotlin's `Result` type for error handling.

## Table of Contents

- [Authentication](#authentication)
- [Events](#events)
- [Registrations](#registrations)
- [Notifications](#notifications)
- [Error Handling](#error-handling)
- [Extending the API](#extending-the-api)

---

## Authentication

### AuthRepository

Location: `app/src/main/java/com/example/sattam_hub/data/repository/AuthRepository.kt`

#### Sign Up

```kotlin
suspend fun signUp(
    email: String,
    password: String,
    fullName: String,
    studentId: String? = null,
    role: String = "STUDENT"
): Result<User>
```

**Parameters:**
- `email`: User's email address
- `password`: User's password (min 8 characters)
- `fullName`: User's full name
- `studentId`: Optional student ID
- `role`: User role (STUDENT, COACH, ADMIN)

**Returns:** `Result<User>` containing the created user or error

**Usage:**
```kotlin
val result = authRepository.signUp(
    email = "student@university.edu",
    password = "SecurePass123",
    fullName = "John Doe",
    studentId = "STU123456",
    role = "STUDENT"
)

result
    .onSuccess { user -> println("Welcome ${user.fullName}") }
    .onFailure { error -> println("Error: ${error.message}") }
```

#### Sign In

```kotlin
suspend fun signIn(email: String, password: String): Result<User>
```

**Parameters:**
- `email`: User's email address
- `password`: User's password

**Returns:** `Result<User>` containing the authenticated user

#### Sign Out

```kotlin
suspend fun signOut(): Result<Unit>
```

**Returns:** `Result<Unit>` indicating success or failure

#### Get Current User

```kotlin
suspend fun getCurrentUser(): Result<User?>
```

**Returns:** `Result<User?>` containing current user or null if not authenticated

#### Update Profile

```kotlin
suspend fun updateUserProfile(user: User): Result<User>
```

**Parameters:**
- `user`: Updated user object

**Returns:** `Result<User>` containing the updated user

---

## Events

### EventRepository

Location: `app/src/main/java/com/example/sattam_hub/data/repository/EventRepository.kt`

#### Get All Events

```kotlin
suspend fun getAllEvents(
    categoryId: String? = null,
    coachId: String? = null,
    status: String = "ACTIVE"
): Result<List<Event>>
```

**Parameters:**
- `categoryId`: Optional filter by category
- `coachId`: Optional filter by coach
- `status`: Event status (default: "ACTIVE")

**Returns:** `Result<List<Event>>` containing list of events

**Usage:**
```kotlin
// Get all active events
val allEvents = eventRepository.getAllEvents()

// Get events by category
val lectures = eventRepository.getAllEvents(categoryId = "category-uuid")

// Get coach's events
val myEvents = eventRepository.getAllEvents(coachId = currentUserId)
```

#### Get Event by ID

```kotlin
suspend fun getEventById(eventId: String): Result<Event>
```

**Parameters:**
- `eventId`: UUID of the event

**Returns:** `Result<Event>` containing the event

#### Get Upcoming Events

```kotlin
suspend fun getUpcomingEvents(limit: Int = 10): Result<List<Event>>
```

**Parameters:**
- `limit`: Maximum number of events to return

**Returns:** `Result<List<Event>>` containing upcoming events sorted by date

#### Search Events

```kotlin
suspend fun searchEvents(query: String): Result<List<Event>>
```

**Parameters:**
- `query`: Search query string

**Returns:** `Result<List<Event>>` containing matching events

**Usage:**
```kotlin
val searchResults = eventRepository.searchEvents("Android")
```

#### Create Event

```kotlin
suspend fun createEvent(event: Event): Result<Event>
```

**Parameters:**
- `event`: Event object to create

**Returns:** `Result<Event>` containing the created event

**Usage:**
```kotlin
val newEvent = Event(
    id = UUID.randomUUID().toString(),
    title = "Android Workshop",
    description = "Learn Android development",
    categoryId = "category-uuid",
    coachId = currentUserId,
    startDate = "2024-12-25T10:00:00Z",
    endDate = "2024-12-25T12:00:00Z",
    location = "Room A101",
    capacity = 30,
    status = "ACTIVE"
)

val result = eventRepository.createEvent(newEvent)
```

#### Update Event

```kotlin
suspend fun updateEvent(event: Event): Result<Event>
```

**Parameters:**
- `event`: Updated event object

**Returns:** `Result<Event>` containing the updated event

#### Delete Event

```kotlin
suspend fun deleteEvent(eventId: String): Result<Unit>
```

**Parameters:**
- `eventId`: UUID of the event to delete

**Returns:** `Result<Unit>` indicating success or failure

#### Get Categories

```kotlin
suspend fun getAllCategories(): Result<List<EventCategory>>
```

**Returns:** `Result<List<EventCategory>>` containing all categories

---

## Registrations

### RegistrationRepository

Location: `app/src/main/java/com/example/sattam_hub/data/repository/RegistrationRepository.kt`

#### Register for Event

```kotlin
suspend fun registerForEvent(
    eventId: String,
    userId: String,
    status: String = "CONFIRMED"
): Result<Registration>
```

**Parameters:**
- `eventId`: UUID of the event
- `userId`: UUID of the user
- `status`: Registration status (CONFIRMED or WAITLISTED)

**Returns:** `Result<Registration>` containing the registration

**Usage:**
```kotlin
// Check if event is full first
val event = eventRepository.getEventById(eventId).getOrNull()
val status = if (event?.isFull() == true) "WAITLISTED" else "CONFIRMED"

val registration = registrationRepository.registerForEvent(
    eventId = eventId,
    userId = currentUserId,
    status = status
)
```

#### Cancel Registration

```kotlin
suspend fun cancelRegistration(registrationId: String): Result<Unit>
```

**Parameters:**
- `registrationId`: UUID of the registration

**Returns:** `Result<Unit>` indicating success or failure

#### Get User Registrations

```kotlin
suspend fun getUserRegistrations(userId: String): Result<List<Registration>>
```

**Parameters:**
- `userId`: UUID of the user

**Returns:** `Result<List<Registration>>` containing user's registrations

#### Get Event Registrations

```kotlin
suspend fun getEventRegistrations(eventId: String): Result<List<Registration>>
```

**Parameters:**
- `eventId`: UUID of the event

**Returns:** `Result<List<Registration>>` containing event registrations

**Usage (for coaches):**
```kotlin
// Get all participants for an event
val registrations = registrationRepository.getEventRegistrations(eventId)

registrations.onSuccess { list ->
    println("Total participants: ${list.size}")
    val confirmed = list.count { it.status == "CONFIRMED" }
    val waitlisted = list.count { it.status == "WAITLISTED" }
    println("Confirmed: $confirmed, Waitlisted: $waitlisted")
}
```

#### Check if User Registered

```kotlin
suspend fun checkIfUserRegistered(
    eventId: String,
    userId: String
): Result<Registration?>
```

**Parameters:**
- `eventId`: UUID of the event
- `userId`: UUID of the user

**Returns:** `Result<Registration?>` containing registration or null

#### Mark Attendance

```kotlin
suspend fun markAttendance(
    registrationId: String,
    attended: Boolean
): Result<Unit>
```

**Parameters:**
- `registrationId`: UUID of the registration
- `attended`: true for ATTENDED, false for NO_SHOW

**Returns:** `Result<Unit>` indicating success or failure

**Usage:**
```kotlin
// Mark student as attended
registrationRepository.markAttendance(registrationId, attended = true)

// Mark as no-show
registrationRepository.markAttendance(registrationId, attended = false)
```

---

## Notifications

### NotificationRepository

Location: `app/src/main/java/com/example/sattam_hub/data/repository/NotificationRepository.kt`

#### Get User Notifications

```kotlin
suspend fun getUserNotifications(
    userId: String,
    unreadOnly: Boolean = false
): Result<List<Notification>>
```

**Parameters:**
- `userId`: UUID of the user
- `unreadOnly`: Filter for unread notifications only

**Returns:** `Result<List<Notification>>` containing notifications

#### Mark as Read

```kotlin
suspend fun markNotificationAsRead(notificationId: String): Result<Unit>
```

**Parameters:**
- `notificationId`: UUID of the notification

**Returns:** `Result<Unit>` indicating success or failure

#### Mark All as Read

```kotlin
suspend fun markAllAsRead(userId: String): Result<Unit>
```

**Parameters:**
- `userId`: UUID of the user

**Returns:** `Result<Unit>` indicating success or failure

#### Create Notification

```kotlin
suspend fun createNotification(
    userId: String,
    title: String,
    message: String,
    type: String,
    eventId: String? = null
): Result<Notification>
```

**Parameters:**
- `userId`: UUID of the recipient
- `title`: Notification title
- `message`: Notification message
- `type`: Notification type (REGISTRATION, REMINDER, CANCELLATION, etc.)
- `eventId`: Optional related event UUID

**Returns:** `Result<Notification>` containing the created notification

**Usage:**
```kotlin
// Send event reminder
notificationRepository.createNotification(
    userId = studentId,
    title = "Event Reminder",
    message = "Your event starts in 1 hour!",
    type = "REMINDER",
    eventId = eventId
)
```

#### Get Unread Count

```kotlin
suspend fun getUnreadCount(userId: String): Result<Int>
```

**Parameters:**
- `userId`: UUID of the user

**Returns:** `Result<Int>` containing unread count

---

## Error Handling

All repository methods return `Result<T>` type. Handle them using:

### Success Case

```kotlin
result.onSuccess { data ->
    // Handle successful result
    println("Success: $data")
}
```

### Failure Case

```kotlin
result.onFailure { error ->
    // Handle error
    println("Error: ${error.message}")
    
    when (error) {
        is HttpException -> {
            // Handle HTTP errors
        }
        is IOException -> {
            // Handle network errors
        }
        else -> {
            // Handle other errors
        }
    }
}
```

### Get or Null

```kotlin
val data = result.getOrNull()
if (data != null) {
    // Use data
}
```

### Get or Default

```kotlin
val data = result.getOrDefault(defaultValue)
```

### Get or Throw

```kotlin
try {
    val data = result.getOrThrow()
} catch (e: Exception) {
    // Handle exception
}
```

---

## Extending the API

### Adding a New Repository Method

1. **Add method to repository:**

```kotlin
suspend fun customQuery(param: String): Result<List<Event>> = try {
    val data = database.from("events")
        .select(columns = Columns.ALL) {
            filter {
                // Your custom filter
            }
        }
        .decodeList<Event>()
    
    Result.success(data)
} catch (e: Exception) {
    Result.failure(e)
}
```

2. **Use in ViewModel:**

```kotlin
fun loadCustomData(param: String) {
    viewModelScope.launch {
        repository.customQuery(param)
            .onSuccess { data ->
                _data.value = data
            }
            .onFailure { error ->
                _error.value = error.message
            }
    }
}
```

### Adding a New Repository

1. **Create repository class:**

```kotlin
@Singleton
class NewRepository @Inject constructor(
    private val database: Postgrest
) {
    suspend fun getData(): Result<List<Data>> = try {
        val data = database.from("table_name")
            .select()
            .decodeList<Data>()
        Result.success(data)
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

2. **Inject into ViewModel:**

```kotlin
@HiltViewModel
class NewViewModel @Inject constructor(
    private val newRepository: NewRepository
) : ViewModel() {
    // Use repository
}
```

---

## Best Practices

1. **Always use Result type** for error handling
2. **Handle both success and failure** cases
3. **Use viewModelScope** for coroutines in ViewModels
4. **Show loading states** during API calls
5. **Provide user feedback** for errors
6. **Cache data** when appropriate
7. **Implement retry logic** for network failures
8. **Use pagination** for large datasets

---

## Common Patterns

### Loading State Pattern

```kotlin
sealed class UiState<out T> {
    object Loading : UiState<Nothing>()
    data class Success<T>(val data: T) : UiState<T>()
    data class Error(val message: String) : UiState<Nothing>()
}
```

### Repository + ViewModel Pattern

```kotlin
// ViewModel
fun loadData() {
    viewModelScope.launch {
        _uiState.value = UiState.Loading
        
        repository.getData()
            .onSuccess { data ->
                _uiState.value = UiState.Success(data)
            }
            .onFailure { error ->
                _uiState.value = UiState.Error(error.message ?: "Unknown error")
            }
    }
}
```

---

## Testing

### Mock Repository

```kotlin
class MockEventRepository : EventRepository {
    override suspend fun getAllEvents(...): Result<List<Event>> {
        return Result.success(listOf(
            Event(/* test data */)
        ))
    }
}
```

### Test ViewModel

```kotlin
@Test
fun `test load events success`() = runTest {
    val viewModel = EventViewModel(mockRepository)
    viewModel.loadEvents()
    
    assert(viewModel.uiState.value is UiState.Success)
}
```

---

**Last Updated**: November 5, 2025

