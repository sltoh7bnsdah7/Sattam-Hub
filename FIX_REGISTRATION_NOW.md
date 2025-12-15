# 🚨 URGENT FIX: User Registration Error

## Error You're Getting
```
new row violates row-level security policy for table "users"
```

---

## ✅ COMPLETE FIX (Follow Both Steps)

### 🔧 **STEP 1: Fix Database (REQUIRED - Do This First!)**

1. **Open Supabase Dashboard**
   - Go to: https://supabase.com/dashboard
   - Open your project: **vidhibcnxsguonppvliz**

2. **Go to SQL Editor**
   - Click **SQL Editor** in the left sidebar
   - Click **+ New Query**

3. **Copy and Paste This ENTIRE Script**
   
   Open the file `database/COMPLETE_FIX.sql` I just created, or copy this:

```sql
-- Drop existing policies to start fresh
DROP POLICY IF EXISTS "Users can view own profile" ON public.users;
DROP POLICY IF EXISTS "Users can update own profile" ON public.users;
DROP POLICY IF EXISTS "Users can insert own profile during registration" ON public.users;
DROP POLICY IF EXISTS "Allow authenticated users to create profile" ON public.users;

-- Create proper policies
CREATE POLICY "Users can view own profile" ON public.users
    FOR SELECT 
    USING (auth.uid() = id);

CREATE POLICY "Users can create own profile" ON public.users
    FOR INSERT 
    WITH CHECK (auth.uid() = id);

CREATE POLICY "Users can update own profile" ON public.users
    FOR UPDATE 
    USING (auth.uid() = id);

-- Enable RLS
ALTER TABLE public.users ENABLE ROW LEVEL SECURITY;

-- Grant permissions
GRANT USAGE ON SCHEMA public TO anon, authenticated;
GRANT ALL ON public.users TO anon, authenticated;
```

4. **Click RUN (▶️)**

5. **Verify Success**
   - You should see "Success. No rows returned"
   - If you see any errors, let me know!

---

### 📱 **STEP 2: Rebuild Your App (REQUIRED)**

The code has been updated to properly handle user creation.

**In Android Studio:**

1. **Clean the project:**
   ```
   Build → Clean Project
   ```

2. **Rebuild:**
   ```
   Build → Rebuild Project
   ```

3. **Or use terminal:**
   ```powershell
   cd C:\Users\sltoh\OneDrive\Desktop\F_PRO
   .\gradlew.bat clean assembleDebug
   ```

4. **Reinstall the app** on your device/emulator

---

## 🧪 **STEP 3: Test Registration**

1. **Open the app**
2. **Click "Sign Up"**
3. **Fill in the form:**
   - Email: test@example.com
   - Password: Test123456
   - Full Name: Test User
   - Student ID: (optional)
   - Role: STUDENT

4. **Click "Create Account"**

✅ **It should work now!**

---

## 🔍 **What Was Wrong?**

### Database Issue:
- ❌ Missing INSERT policy on users table
- ✅ Fixed: Added policy to allow users to create their profile

### Code Issue:
- ❌ Using wrong user ID from signup result
- ✅ Fixed: Now using authenticated session's user ID

---

## ❓ **Still Not Working?**

### Check Database Policies

Run this in SQL Editor to verify policies:

```sql
SELECT policyname, cmd
FROM pg_policies 
WHERE tablename = 'users';
```

**You should see:**
- ✅ `Users can view own profile` (SELECT)
- ✅ `Users can create own profile` (INSERT) ← **Most important!**
- ✅ `Users can update own profile` (UPDATE)

### Check Supabase Auth Settings

1. Go to **Authentication → Settings** in Supabase
2. Make sure **Email** provider is enabled
3. Check **Email Confirmations**:
   - If "Enable email confirmations" is ON, users need to verify email first
   - For testing, you can turn this OFF

### Verify Credentials

Check `local.properties`:
```properties
SUPABASE_URL=https://vidhibcnxsguonppvliz.supabase.co
SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Make sure these match your Supabase project!

---

## 🐛 **Debugging Tips**

### Get More Details

Add logging to see what's happening:

In `AuthRepository.kt` (already updated), you can add:

```kotlin
Log.d("AuthRepository", "Signing up user: $email")
Log.d("AuthRepository", "User ID: $userId")
```

### Check Supabase Logs

1. Go to Supabase Dashboard → **Logs**
2. Check **Database** logs for RLS errors
3. Look for the specific INSERT that's failing

### Test with Supabase API

In Supabase Dashboard → **API Docs**, try:
1. Create a test user via the UI
2. Try inserting into users table manually

---

## 📊 **Expected Flow**

Here's what should happen:

1. **User fills registration form** ✅
2. **App calls `auth.signUpWith(Email)`** ✅
   - Creates user in `auth.users`
   - Returns authenticated session
3. **App gets `auth.currentUserOrNull()?.id`** ✅
   - Gets user ID from session
4. **App inserts into `public.users`** ✅
   - RLS policy checks: `auth.uid() = id`
   - Should pass because user is authenticated
5. **Profile created successfully!** ✅

---

## 🎯 **Quick Checklist**

Before you test:

- [ ] Ran the SQL script in Supabase SQL Editor
- [ ] Saw "Success" message
- [ ] Cleaned and rebuilt the Android project
- [ ] Reinstalled the app
- [ ] Verified Supabase credentials in `local.properties`
- [ ] Email provider is enabled in Supabase Auth settings

---

## 💬 **If This Still Doesn't Work**

Send me:

1. **Screenshot of SQL query result** (the verify policies query)
2. **Error message from Android Studio Logcat**
3. **Supabase Auth settings** (screenshot of Email provider settings)

I'll help you debug further!

---

## ✅ **Summary**

**Two fixes applied:**

1. ✅ **Database**: Added INSERT policy for users table
2. ✅ **Code**: Fixed user ID retrieval from auth session

**Action required:**
1. Run the SQL in Supabase
2. Rebuild the app
3. Test registration

**This WILL fix your registration error!** 🎉

---

**Created**: November 5, 2025  
**Status**: Ready to test

