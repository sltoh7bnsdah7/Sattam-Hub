# Setup Instructions for Sattam Hub (University Event Management System)

## 🔐 Configuring Supabase Credentials

To connect the app to your Supabase backend, you need to add your credentials to the `local.properties` file.

### Step 1: Open local.properties

The file is located in the root directory of your project: `local.properties`

### Step 2: Add Your Supabase Credentials

Add these two lines to the end of the `local.properties` file:

```properties
SUPABASE_URL=your_supabase_project_url_here
SUPABASE_ANON_KEY=your_supabase_anon_key_here
```

Replace:
- `your_supabase_project_url_here` with your actual Supabase project URL (e.g., `https://xxxxx.supabase.co`)
- `your_supabase_anon_key_here` with your actual Supabase anonymous key

### Example:

```properties
sdk.dir=C\:\\Users\\sltoh\\AppData\\Local\\Android\\Sdk
SUPABASE_URL=https://abcdefghijklm.supabase.co
SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImFiY2RlZmdoaWprbG0iLCJyb2xlIjoiYW5vbiIsImlhdCI6MTYxMjAxMjM0NSwiZXhwIjoxOTI3NTg4MzQ1fQ.example_key_here
```

### 🔒 Security Note

The `local.properties` file is already included in `.gitignore`, so your credentials **will not** be committed to version control. This keeps your sensitive data secure.

### Where to Find Your Supabase Credentials

1. Go to your Supabase Dashboard: https://supabase.com/dashboard
2. Select your project
3. Go to Settings → API
4. Copy:
   - **Project URL** → Use as `SUPABASE_URL`
   - **Project API keys → anon public** → Use as `SUPABASE_ANON_KEY`

---

## 📊 Supabase Database Setup

You'll need to create the following tables in your Supabase database. The app will automatically create these through migrations, or you can create them manually.

### Required Tables:
1. **users** - User profiles (students, coaches, admins)
2. **events** - Event information
3. **registrations** - Event registrations
4. **notifications** - User notifications
5. **categories** - Event categories
6. **attendance** - Attendance records

Detailed SQL schema will be provided in the `database/schema.sql` file.

---

## 🚀 Running the App

After adding your Supabase credentials:

1. Sync Gradle: File → Sync Project with Gradle Files
2. Build the project
3. Run on emulator or physical device

---

## 📱 Features Included

### Student Features:
- Browse and discover events
- Register for events
- View personal dashboard
- Receive notifications
- Track attendance history

### Coach Features:
- Create and manage events
- Track participants
- Mark attendance
- View analytics

### Admin Features:
- System-wide event management
- User management
- Reports and analytics
- System configuration

---

## 🛠️ Tech Stack

- **Frontend**: Jetpack Compose with Material 3
- **Backend**: Supabase (PostgreSQL, Auth, Realtime, Storage)
- **Architecture**: MVVM with Clean Architecture
- **Dependency Injection**: Hilt
- **Navigation**: Jetpack Navigation Compose
- **Image Loading**: Coil
- **Local Storage**: DataStore

---

## 📞 Support

If you encounter any issues during setup, please check:
1. Supabase credentials are correctly copied
2. No extra spaces in the credentials
3. Gradle sync completed successfully
4. Internet connection is active

---

**Last Updated**: November 5, 2025

