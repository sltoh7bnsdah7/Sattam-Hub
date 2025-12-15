# Translation Implementation Guide

## ✅ Completed Updates

The following files have been updated to use string resources:
1. ✅ `GeneralSettingsScreen.kt` - All labels and titles
2. ✅ `CreateEventScreen.kt` - Headers, buttons, and error messages
3. ✅ `CoachUi.kt` - Placeholders, labels, and navigation items

## 📋 Remaining Files to Update

### High Priority (Most Visible Screens)

#### 1. `CoachDashboardScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/coach/CoachDashboardScreen.kt`

**Strings to replace:**
- `"Published Events"` → `stringResource(R.string.published_events)`
- `"No published events yet"` → `stringResource(R.string.no_published_events)`
- `"Tap the plus button to create your first event"` → `stringResource(R.string.create_first_event)`
- `"Drafts"` → `stringResource(R.string.drafts)`
- `"Keep working on your ideas"` → `stringResource(R.string.keep_working)`
- `"No drafts yet"` → `stringResource(R.string.no_drafts)`
- `"Save a draft to see it here"` → `stringResource(R.string.save_draft_to_see)`
- `"Notifications"` → `stringResource(R.string.notifications_title)`
- `"System reminders and alerts"` → `stringResource(R.string.system_reminders)`
- `"No notifications yet"` → `stringResource(R.string.no_notifications)`
- `"You're all caught up"` → `stringResource(R.string.all_caught_up)`
- `"Edit Profile"` → `stringResource(R.string.edit_profile)`
- `"General Settings"` → `stringResource(R.string.general_settings)`
- `"Help Center"` → `stringResource(R.string.help_center)`
- `"Log out"` → `stringResource(R.string.log_out)`
- `"Developed by Sattam Hub Team © 2025"` → `stringResource(R.string.developed_by)`

**Add imports:**
```kotlin
import androidx.compose.ui.res.stringResource
import com.example.sattam_hub.R
```

#### 2. `EventDetailScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/events/EventDetailScreen.kt`

**Strings to replace:**
- `"Event Details"` → `stringResource(R.string.event_details)`
- `"Description"` → `stringResource(R.string.description)`
- `"Organizer"` → `stringResource(R.string.organizer)`
- `"Location"` → `stringResource(R.string.location)`
- `"Requirements"` → `stringResource(R.string.requirements)`
- `"No requirements"` → `stringResource(R.string.no_requirements)`
- `"Already Registered"` → `stringResource(R.string.already_registered)`
- `"Join Waitlist"` → `stringResource(R.string.join_waitlist)`
- `"Register"` → `stringResource(R.string.register)`
- `"Go to Home"` → `stringResource(R.string.go_to_home)`
- `"Registration Confirmed"` → `stringResource(R.string.registration_confirmed)`
- `"You're registered for this event. See your updates on Home."` → `stringResource(R.string.registered_for_event)`

#### 3. `HomeScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/home/HomeScreen.kt`

**Strings to replace:**
- `"Filter"` → `stringResource(R.string.filter)`
- `"Event Type"` → `stringResource(R.string.event_type)`
- `"Date"` → `stringResource(R.string.date)`
- `"Mode"` → `stringResource(R.string.mode)`
- `"Apply"` → `stringResource(R.string.apply)`
- `"Reset"` → `stringResource(R.string.reset)`
- Search placeholder text

#### 4. `ProfileScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/profile/ProfileScreen.kt`

**Strings to replace:**
- `"Edit Profile"` → `stringResource(R.string.edit_profile)`
- `"General Settings"` → `stringResource(R.string.general_settings)`
- `"Help Center"` → `stringResource(R.string.help_center)`
- `"Log out"` → `stringResource(R.string.log_out)`
- `"Developed by Sattam Hub Team © 2025"` → `stringResource(R.string.developed_by)`

#### 5. `EditProfileScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/student/EditProfileScreen.kt`

**Strings to replace:**
- `"Change Photo"` → `stringResource(R.string.change_photo)`
- `"Full Name"` → `stringResource(R.string.full_name)`
- `"Email Address"` → `stringResource(R.string.email_address)`
- `"Phone Number"` → `stringResource(R.string.phone_number)`
- `"Password"` → `stringResource(R.string.password)`
- `"Save Changes"` → `stringResource(R.string.save_changes)`
- `"Cancel"` → `stringResource(R.string.cancel)`

#### 6. `MyEventsScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/student/MyEventsScreen.kt`

**Strings to replace:**
- `"My Events"` → `stringResource(R.string.my_events)`
- `"Explore Events"` → `stringResource(R.string.explore_events)`

#### 7. `NotificationScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/notifications/NotificationScreen.kt`

**Strings to replace:**
- `"Notifications"` → `stringResource(R.string.notifications_title)`

### Medium Priority (Admin & Other Screens)

#### 8. `AdminDashboardScreen.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/admin/AdminDashboardScreen.kt`

**Strings to replace:**
- `"Welcome, $currentUserName"` → `stringResource(R.string.welcome, currentUserName)`
- `"System Overview & Statistics"` → `stringResource(R.string.system_overview)`
- `"Total Users"` → `stringResource(R.string.total_users)`
- `"Active Events"` → `stringResource(R.string.active_events)`
- `"Pending Requests"` → `stringResource(R.string.pending_requests)`
- `"Students"` → `stringResource(R.string.students)`
- `"Trainers"` → `stringResource(R.string.trainers)`

#### 9. `AdminUi.kt` and other Admin screens
Update all admin-related strings using the same pattern.

#### 10. `StudentUi.kt`
**Location**: `app/src/main/java/com/example/sattam_hub/ui/student/StudentUi.kt`

**Strings to replace:**
- `"Home"` → `stringResource(R.string.home)`
- `"My Events"` → `stringResource(R.string.my_events)`
- `"Notifications"` → `stringResource(R.string.notifications_title)`
- `"Profile"` → `stringResource(R.string.profile)`

## 🔧 Implementation Pattern

### Step 1: Add Imports
```kotlin
import androidx.compose.ui.res.stringResource
import com.example.sattam_hub.R
```

### Step 2: Replace Hardcoded Strings
**Before:**
```kotlin
Text("Home")
title = "Create Event"
label = "Save"
```

**After:**
```kotlin
Text(stringResource(R.string.home))
title = stringResource(R.string.create_event)
label = stringResource(R.string.save)
```

### Step 3: For Strings with Parameters
**Before:**
```kotlin
Text("Welcome, $name")
```

**After:**
```kotlin
Text(stringResource(R.string.welcome, name))
```

## ✅ Verification Checklist

After updating each file:
- [ ] Added necessary imports
- [ ] Replaced all hardcoded English strings
- [ ] Tested in English (default)
- [ ] Tested in Arabic (RTL layout)
- [ ] Verified text fits UI space
- [ ] Checked for any remaining hardcoded strings

## 🎯 Quick Update Script Pattern

For each file:
1. Add imports at the top
2. Find all `Text("...")` and replace with `Text(stringResource(R.string.key))`
3. Find all `title = "..."` and replace with `title = stringResource(R.string.key)`
4. Find all `label = "..."` and replace with `label = stringResource(R.string.key)`
5. Find all `placeholder = "..."` and replace with `placeholder = stringResource(R.string.key)`

## 📝 Notes

- All string resources are already defined in both `values/strings.xml` (English) and `values-ar/strings.xml` (Arabic)
- The app automatically switches between languages based on user selection
- RTL layout is handled automatically when Arabic is selected
- No need to restart the app - language changes are instant

## 🚀 Next Steps

1. Update the remaining high-priority screens listed above
2. Test the app in both English and Arabic
3. Verify all text displays correctly in RTL layout
4. Check for any overflow or layout issues with Arabic text
5. Update any remaining admin or less-visible screens

---

**Status**: Core translation system is complete. Remaining work is updating individual screen files to use string resources.
