# Backend Localization Implementation Note

## Current Status

The Android app now has full frontend localization support with:
- Language preference persistence (DataStore)
- RTL layout support for Arabic
- Dynamic UI language switching
- Language state management (LanguageViewModel)

## Backend Accept-Language Header

**Important:** Supabase Kt (the Kotlin client library) doesn't easily support adding custom headers like `Accept-Language` to all requests through the client configuration.

### Current Implementation

The app maintains the selected language in `LanguageHeaderProvider`, which can be accessed to get the current language code. However, Supabase Kt's Postgrest API doesn't expose a straightforward way to add headers to all requests.

### Recommended Solutions

#### Option 1: Backend Database Functions (Recommended)
Create PostgreSQL functions that return localized messages based on a language parameter:

```sql
CREATE OR REPLACE FUNCTION get_localized_message(
    message_key TEXT,
    language_code TEXT DEFAULT 'en'
) RETURNS TEXT AS $$
BEGIN
    RETURN CASE language_code
        WHEN 'ar' THEN
            CASE message_key
                WHEN 'event_created' THEN 'تم إنشاء الفعالية بنجاح'
                WHEN 'event_updated' THEN 'تم تحديث الفعالية بنجاح'
                WHEN 'registration_success' THEN 'تم التسجيل بنجاح'
                -- Add more Arabic translations
                ELSE message_key
            END
        ELSE
            CASE message_key
                WHEN 'event_created' THEN 'Event created successfully'
                WHEN 'event_updated' THEN 'Event updated successfully'
                WHEN 'registration_success' THEN 'Registration successful'
                -- Add more English translations
                ELSE message_key
            END
    END;
END;
$$ LANGUAGE plpgsql;
```

#### Option 2: Translation Table
Create a translations table in the database:

```sql
CREATE TABLE translations (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    key TEXT NOT NULL,
    language_code TEXT NOT NULL,
    value TEXT NOT NULL,
    UNIQUE(key, language_code)
);

-- Insert translations
INSERT INTO translations (key, language_code, value) VALUES
    ('event_created', 'en', 'Event created successfully'),
    ('event_created', 'ar', 'تم إنشاء الفعالية بنجاح'),
    ('event_updated', 'en', 'Event updated successfully'),
    ('event_updated', 'ar', 'تم تحديث الفعالية بنجاح');
```

#### Option 3: Supabase Edge Functions
If you're using Supabase Edge Functions, you can read the `Accept-Language` header there and return localized responses.

### For Now

The frontend is fully prepared for localization. When you implement backend localization:

1. The `LanguageHeaderProvider` class maintains the current language
2. You can access it via: `languageHeaderProvider.getAcceptLanguageHeader()` which returns "ar" or "en"
3. Update your backend to read the `Accept-Language` header from requests
4. Return localized error messages and responses based on the header

### Testing

To test backend localization:
1. Set the app language to Arabic in General Settings
2. Make API calls (create event, register, etc.)
3. Verify that error messages and success notifications come back in Arabic
4. Check that the backend receives the `Accept-Language: ar` header

## Next Steps

1. Implement one of the backend localization solutions above
2. Update error handling in repositories to use localized messages
3. Test with both English and Arabic languages
4. Consider adding more languages in the future
