# Quick Start Guide - Sattam Hub

## ⚡ Get Started in 5 Minutes

### Step 1: Add Your Supabase Credentials

Open `local.properties` and add these two lines at the end:

```properties
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key-here
```

💡 **Where to find these?**
- Go to https://supabase.com/dashboard
- Select your project
- Click Settings → API
- Copy the values

### Step 2: Set Up Supabase Database

1. In your Supabase dashboard, go to **SQL Editor**
2. Create a new query
3. Copy the entire contents of `database/schema.sql`
4. Paste and run it
5. ✅ Your database is ready!

### Step 3: Sync & Build

In Android Studio:
1. Click **File → Sync Project with Gradle Files**
2. Wait for sync to complete (may take 2-3 minutes first time)
3. Click the **Run** button (▶️)

### Step 4: Create Your First Account

When the app launches:
1. Click **"Don't have an account? Sign Up"**
2. Fill in your details:
   - Full Name
   - Email
   - Student ID (optional)
   - Role: Choose STUDENT, COACH, or ADMIN
   - Password
3. Click **Create Account**

### Step 5: Test the Features

#### As a Student:
- Browse events on the home screen
- Click "Explore" to see all events
- Tap an event to view details
- Click "Register Now" to join

#### As a Coach:
- Tap the dashboard icon in bottom nav
- Click the ➕ button to create an event
- Fill in event details
- Manage participants and mark attendance

#### As an Admin:
- Access all system features
- Manage users and events
- View comprehensive reports

## 🎯 Key Screens Navigation

```
Login Screen
    ↓
Home Screen (Dashboard)
    ├── Explore → Event List → Event Details
    ├── My Events → View Registrations
    ├── Notifications → See Updates
    └── Profile → Account Settings

Coach Dashboard
    ├── Create Event
    ├── My Events
    └── Manage Participants
```

## 🔧 Troubleshooting

### "Failed to load events"
- ✅ Check internet connection
- ✅ Verify Supabase credentials in `local.properties`
- ✅ Ensure database schema is set up correctly

### "Sign in failed"
- ✅ Make sure you've created an account first
- ✅ Check email and password are correct
- ✅ Verify Supabase Auth is enabled

### Build errors
- ✅ Sync Gradle files
- ✅ Clean project: `Build → Clean Project`
- ✅ Rebuild: `Build → Rebuild Project`

## 📝 Sample Test Data

Want to test with sample data? Run this in Supabase SQL Editor:

```sql
-- Create a sample coach user (after signing up with this email)
-- Replace 'user-uuid-here' with actual UUID from auth.users
INSERT INTO public.users (id, email, full_name, role) VALUES 
('your-uuid', 'coach@example.com', 'John Coach', 'COACH');

-- Create sample events
INSERT INTO public.events (
    title, 
    description, 
    category_id, 
    coach_id, 
    start_date, 
    end_date, 
    location, 
    capacity
) VALUES 
(
    'Introduction to Android Development',
    'Learn the basics of Android development with Kotlin',
    (SELECT id FROM public.categories WHERE name = 'Training Course' LIMIT 1),
    'your-coach-uuid-here',
    NOW() + INTERVAL '7 days',
    NOW() + INTERVAL '7 days' + INTERVAL '2 hours',
    'Computer Lab A101',
    30
);
```

## 🎨 Customization

### Change App Name
Edit `app/src/main/res/values/strings.xml`:
```xml
<string name="app_name">Your Custom Name</string>
```

### Change App Colors
Edit `app/src/main/java/com/example/sattam_hub/ui/theme/Color.kt`

### Change Package Name
Use Android Studio's refactor tool:
`Right-click package → Refactor → Rename`

## 📱 Test User Accounts

For testing, create these accounts:

1. **Student Account**
   - Email: student@test.com
   - Role: STUDENT

2. **Coach Account**
   - Email: coach@test.com
   - Role: COACH

3. **Admin Account**
   - Email: admin@test.com
   - Role: ADMIN

## 🚀 Next Steps

- ✅ Customize the UI theme
- ✅ Add your university logo
- ✅ Configure notification templates
- ✅ Set up email notifications in Supabase
- ✅ Deploy to production

## 💡 Pro Tips

1. **Testing**: Use Android Studio's emulator for quick testing
2. **Debugging**: Check Logcat for detailed error messages
3. **Database**: Use Supabase Table Editor for easy data management
4. **Performance**: Test with at least 50 events for realistic performance

## 📞 Need Help?

- 📖 Full documentation: See `README.md`
- 🔧 Setup details: See `SETUP_INSTRUCTIONS.md`
- 🗄️ Database: See `database/schema.sql`
- 💬 Issues: Open a GitHub issue

---

**Happy coding! 🎉**

