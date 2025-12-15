# Reports & Analytics Implementation

This document describes the implementation of the 4 export reports for the Reports & Analytics page.

## Overview

Two implementations are provided:
1. **Local Generation (Current)**: Reports are generated directly on the Android device and saved to Downloads
2. **API-Based (Optional)**: Supabase Edge Functions serve reports via HTTP with proper headers

## Report Types

### 1. Event Participation Report
**Columns:**
- Student Name
- Event Name
- Registration Date
- Attendance Status

**Data Source:** All registrations across all events

### 2. User Activity Report
**Columns:**
- User Name
- Role (Student/Uni/Admin)
- Date Joined
- Last Login
- Total Events Registered

**Data Source:** All users with their registration statistics

### 3. Event Statistics Report
**Columns:**
- Event Title
- Total Seats
- Seats Taken
- % Occupancy
- Status (Active/Finished)

**Data Source:** All events with aggregated registration data

### 4. System Overview Report
**Columns:**
- Metric Name
- Value

**Metrics:**
- Total Users
- Total Events
- Total Active Events
- Total Completed Events

## Current Implementation (Local Generation)

The current implementation generates CSV files locally on the Android device:

**Files Modified:**
- `app/src/main/java/com/example/sattam_hub/data/repository/ReportRepository.kt`
- `app/src/main/java/com/example/sattam_hub/ui/admin/AdminReportsScreen.kt`
- `app/src/main/java/com/example/sattam_hub/ui/admin/AdminViewModel.kt`

**Usage:**
- Each "Export" button in `AdminReportsScreen` calls `downloadReport()` function
- The function fetches data from the repository, generates CSV, and saves to Downloads folder
- Files are saved with format: `{report_name}_report_{YYYYMMDD}.csv`

## Optional: API-Based Implementation (Supabase Edge Functions)

If you want to serve reports via HTTP API endpoints with proper headers, deploy the Edge Function:

### Deployment Steps

1. **Install Supabase CLI** (if not already installed):
   ```bash
   npm install -g supabase
   ```

2. **Login to Supabase**:
   ```bash
   supabase login
   ```

3. **Link your project**:
   ```bash
   supabase link --project-ref your-project-ref
   ```

4. **Deploy the function**:
   ```bash
   supabase functions deploy export-report
   ```

### API Endpoints

**Base URL:** `https://your-project.supabase.co/functions/v1/export-report`

**Request:**
```json
POST /functions/v1/export-report
Headers:
  Authorization: Bearer YOUR_ANON_KEY
  Content-Type: application/json
Body:
  {
    "report_type": "event_participation" | "user_activity" | "event_statistics" | "system_overview",
    "format": "csv" // optional, defaults to csv
  }
```

**Response:**
- Content-Type: `text/csv`
- Content-Disposition: `attachment; filename="{report_name}_{date}.csv"`
- Body: CSV file content

### Example Usage (cURL)

```bash
curl -X POST https://your-project.supabase.co/functions/v1/export-report \
  -H "Authorization: Bearer YOUR_ANON_KEY" \
  -H "Content-Type: application/json" \
  -d '{"report_type": "event_participation"}' \
  --output event_participation.csv
```

### Integrating Edge Functions in Android App

If you deploy the Edge Functions and want to use API-based downloads, you can create a helper function:

```kotlin
suspend fun downloadReportFromAPI(
    reportType: String,
    context: Context,
    supabaseClient: SupabaseClient
): Result<Unit> = try {
    val response = supabaseClient.functions
        .invoke("export-report") {
            body = mapOf("report_type" to reportType)
        }
    
    val filename = "${reportType}_report_${todayStamp()}.csv"
    val bytes = response.bodyBytes
    
    DownloadSaver.saveToDownloads(
        context = context,
        fileName = filename,
        mimeType = "text/csv",
        bytes = bytes
    )
    
    Result.success(Unit)
} catch (e: Exception) {
    Result.failure(e)
}
```

## File Locations

**Edge Function:**
- `supabase/functions/export-report/index.ts`

**Android Implementation:**
- `app/src/main/java/com/example/sattam_hub/data/repository/ReportRepository.kt`
- `app/src/main/java/com/example/sattam_hub/ui/admin/AdminReportsScreen.kt`
- `app/src/main/java/com/example/sattam_hub/ui/admin/AdminViewModel.kt`
- `app/src/main/java/com/example/sattam_hub/util/export/CsvExporter.kt`
- `app/src/main/java/com/example/sattam_hub/util/export/DownloadSaver.kt`

## Notes

- The local implementation currently only supports CSV format
- Edge Functions can be extended to support Excel (.xlsx) format if needed
- Both implementations use the same database queries and data transformations
- The local implementation is more efficient for Android as it doesn't require a round-trip to the server
- Edge Functions are useful if you want to share reports with other clients (web app, etc.)
