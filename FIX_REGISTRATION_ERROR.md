# 🔧 Fix User Registration Error

## Problem
When creating a new account, you get this error:
```
new row violates row-level security policy for table "users"
```

## Cause
The `users` table has Row-Level Security (RLS) enabled but is missing an **INSERT policy** that allows new users to create their profile during registration.

---

## ✅ Quick Fix (2 Steps)

### Step 1: Go to Supabase SQL Editor
1. Open your Supabase Dashboard: https://supabase.com/dashboard
2. Select your project: **vidhibcnxsguonppvliz**
3. Click **SQL Editor** in the left sidebar
4. Click **New Query**

### Step 2: Run This SQL Command

Copy and paste this SQL and click **RUN**:

```sql
-- Add INSERT policy for user registration
CREATE POLICY "Users can insert own profile during registration" ON public.users
    FOR INSERT 
    WITH CHECK (auth.uid() = id);
```

That's it! ✅

---

## 🧪 Test the Fix

1. Go back to your app
2. Try creating a new account again
3. Fill in all the registration details
4. Click **Create Account**
5. ✅ It should work now!

---

## 📝 What This Does

This SQL command creates a Row-Level Security policy that:
- ✅ Allows authenticated users to INSERT their own profile
- ✅ Ensures users can only create a profile with their own user ID
- ✅ Prevents users from creating profiles for other users

The policy checks: `auth.uid() = id`
- `auth.uid()` = The ID of the currently authenticated user (from Supabase Auth)
- `id` = The ID being inserted into the users table
- They must match for the INSERT to succeed

---

## 🔍 Verify the Fix

To confirm the policy was created, run this in SQL Editor:

```sql
SELECT policyname, cmd, with_check
FROM pg_policies 
WHERE tablename = 'users';
```

You should see:
- `Users can view own profile` (SELECT)
- `Users can insert own profile during registration` (INSERT) ← **New!**
- `Users can update own profile` (UPDATE)

---

## 🛡️ Security Notes

This policy is **secure** because:
- ✅ Users can only create their OWN profile (auth.uid() must match id)
- ✅ Cannot create profiles for other users
- ✅ Must be authenticated to create a profile
- ✅ Follows Supabase security best practices

---

## 💡 Alternative: Less Restrictive Policy (Not Recommended)

If you want ANY authenticated user to be able to create a profile (less secure):

```sql
-- Drop the existing policy first
DROP POLICY IF EXISTS "Users can insert own profile during registration" ON public.users;

-- Create a less restrictive policy
CREATE POLICY "Allow authenticated users to create profile" ON public.users
    FOR INSERT 
    WITH CHECK (auth.uid() IS NOT NULL);
```

**Note**: This is less secure because users could potentially create profiles with any user ID.

---

## ❓ Troubleshooting

### Still getting the error?
1. **Clear app cache**: Uninstall and reinstall the app
2. **Check SQL ran successfully**: Look for green success message
3. **Verify policy exists**: Run the verification query above
4. **Check credentials**: Make sure you're using the correct Supabase project

### Error: "policy already exists"
This means the policy is already created. You're good to go! ✅

### Can't access SQL Editor?
Make sure you're logged into the correct Supabase account and project.

---

## 📚 Related Documentation

- [Supabase Row Level Security](https://supabase.com/docs/guides/auth/row-level-security)
- [Supabase Policies](https://supabase.com/docs/guides/auth/row-level-security#policies)

---

**Last Updated**: November 5, 2025

