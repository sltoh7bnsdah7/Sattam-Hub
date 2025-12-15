# Sattam Hub - University Event Management System

A comprehensive Android application built with Jetpack Compose and Supabase for managing university events, enabling seamless coordination between students, coaches, and administrators.

## 📱 Features

### For Students
- **Event Discovery**: Browse and search all upcoming campus events
- **Easy Registration**: One-click registration with instant confirmation
- **Personal Dashboard**: View all registered events in one place
- **Real-time Notifications**: Get alerts for event reminders, changes, and updates
- **Event History**: Track attendance and completed events
- **Waitlist Support**: Automatic waitlist management for popular events

### For Coaches/Instructors
- **Event Management**: Create, edit, and manage events
- **Participant Tracking**: View all registered participants
- **Attendance Marking**: Easy check-in system for event attendance
- **Analytics Dashboard**: View event statistics and engagement metrics
- **Bulk Communications**: Send updates to all registered participants

### For Administrators
- **System Overview**: Monitor all campus events and activities
- **User Management**: Manage student, coach, and admin accounts
- **Comprehensive Reports**: Generate attendance and engagement reports
- **Category Management**: Organize events with customizable categories
- **System Configuration**: Configure notifications and business rules

## 🛠️ Tech Stack

- **Frontend**: Jetpack Compose with Material 3 Design
- **Backend**: Supabase (PostgreSQL, Authentication, Realtime, Storage)
- **Architecture**: MVVM with Clean Architecture principles
- **Dependency Injection**: Hilt
- **Navigation**: Jetpack Navigation Compose
- **Image Loading**: Coil
- **Local Storage**: DataStore Preferences
- **Language**: Kotlin

## 📋 Prerequisites

- Android Studio Hedgehog or newer
- Android SDK 24+ (Android 7.0+)
- Supabase account
- JDK 11 or higher

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/sattam-hub.git
cd sattam-hub
```

### 2. Set Up Supabase

1. Create a new project on [Supabase](https://supabase.com)
2. Go to SQL Editor and run the schema from `database/schema.sql`
3. Enable Email authentication in Authentication → Providers
4. Copy your project URL and anon key from Settings → API

### 3. Configure Credentials

Open `local.properties` in the root directory and add:

```properties
SUPABASE_URL=your_supabase_project_url
SUPABASE_ANON_KEY=your_supabase_anon_key
```

**Important**: The `local.properties` file is gitignored, so your credentials will never be committed to version control.

### 4. Sync and Build

1. Open the project in Android Studio
2. Sync Gradle files: `File → Sync Project with Gradle Files`
3. Wait for dependencies to download
4. Build the project: `Build → Make Project`

### 5. Run the Application

1. Connect an Android device or start an emulator
2. Click Run (▶️) or press `Shift + F10`
3. The app will install and launch on your device

## 📁 Project Structure

```
sattam_hub/
├── app/
│   └── src/main/java/com/example/sattam_hub/
│       ├── data/
│       │   ├── model/          # Data models
│       │   └── repository/     # Data repositories
│       ├── di/                 # Dependency injection modules
│       ├── navigation/         # Navigation setup
│       └── ui/                 # UI screens and components
│           ├── auth/           # Authentication screens
│           ├── events/         # Event screens
│           ├── student/        # Student-specific screens
│           ├── coach/          # Coach-specific screens
│           ├── profile/        # Profile screen
│           ├── notifications/  # Notifications screen
│           └── theme/          # App theming
├── database/
│   └── schema.sql             # Supabase database schema
├── SETUP_INSTRUCTIONS.md      # Detailed setup guide
└── README.md                  # This file
```

## 🗄️ Database Schema

The application uses the following main tables:

- **users**: User profiles (students, coaches, admins)
- **categories**: Event categories
- **events**: Event information
- **registrations**: User event registrations
- **attendance**: Attendance records
- **notifications**: User notifications

For detailed schema information, see `database/schema.sql`.

## 🔐 Security Features

- **Row Level Security (RLS)**: Database-level access control
- **Secure Authentication**: Supabase Auth with JWT tokens
- **Encrypted Storage**: Sensitive data encrypted at rest
- **Environment Variables**: Credentials stored securely in `local.properties`
- **API Key Protection**: Anon key for client-side operations only

## 📱 User Roles

### Student
- Browse and register for events
- View personal dashboard
- Receive notifications
- Track attendance history

### Coach
- Create and manage events
- View participant lists
- Mark attendance
- Access event analytics

### Administrator
- Full system access
- User management
- Generate reports
- Configure system settings

## 🎨 Design System

The app follows Material 3 Design guidelines with:
- Dynamic color theming
- Responsive layouts for various screen sizes
- Consistent spacing and typography
- Accessible components (WCAG 2.1 compliant)

## 📊 Key Features Implementation

### Event Registration Flow
1. Student browses events
2. Clicks "Register" on event detail
3. System checks capacity
4. Assigns "CONFIRMED" or "WAITLISTED" status
5. Sends confirmation notification
6. Updates event registration count

### Attendance Tracking
1. Coach opens event participant list
2. Marks attendees as "ATTENDED" or "NO_SHOW"
3. System updates registration status
4. Creates attendance record
5. Updates event statistics

### Notification System
- Event reminders (24 hours and 1 hour before)
- Registration confirmations
- Event cancellations or modifications
- Waitlist promotions
- Capacity alerts for coaches

## 🧪 Testing

### Run Unit Tests
```bash
./gradlew test
```

### Run Instrumented Tests
```bash
./gradlew connectedAndroidTest
```

## 📦 Building Release APK

```bash
./gradlew assembleRelease
```

The APK will be generated at: `app/build/outputs/apk/release/app-release.apk`

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- **Supabase** for the excellent backend platform
- **Jetpack Compose** team for the modern UI toolkit
- **Material Design** for the design system
- **Hilt** for dependency injection

## 📞 Support

For issues and questions:
- Open an issue on GitHub
- Contact: [turki9.abdullah9@gmail.com]

## 🗺️ Roadmap

### Phase 2 (Planned)
- [ ] Advanced search and filtering
- [ ] Calendar integration (Google Calendar, Outlook)
- [ ] QR code check-in
- [ ] Feedback and rating system

### Phase 3 (Future)

- [ ] AI-powered event recommendations
- [ ] Mobile app 
- [ ] Certificate generation



**Built with ❤️ for university communities**

**Version**: 1.0.0  
**Last Updated**: November 5, 2025



