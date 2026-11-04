# CareDose

**A medication reminder app for individuals and family caregivers.** Set up a schedule for each patient, see what is due today, and record when a dose was taken, given, missed, or snoozed.

This public showcase contains product documentation and iPhone beta screenshots. The application source and build files are private.

[Feature tour](#feature-tour) · [Supabase integration](#supabase-integration) · [Beta status](#beta-status)

## Feature tour

| Feature | iPhone beta screenshot |
| --- | --- |
| **Today and dose progress**<br>See the active patient's scheduled doses and how many are complete. Cards show the scheduled time, dose instructions, and the current status. Open a dose or add a medication from the same screen. | <img src="screenshots/today.jpg" width="260" alt="Today screen with a completed dose progress bar and two medication cards"> |
| **Patient profiles**<br>Keep separate medication schedules for yourself and the people you care for. Add or remove profiles and choose whose day to view. | <img src="screenshots/patients.jpg" width="260" alt="Patient picker with two profiles, medication counts, and an add patient button"> |
| **Medication details**<br>Review a medication's dose, instructions, active or paused status, repeat interval, start time, duration, and notes. Medication management also supports editing, pausing, and removing an entry. | <img src="screenshots/medication-details.jpg" width="260" alt="Medication details with dose instructions, repeat schedule, and personal notes"> |
| **Add a medication**<br>Enter the medication name and optionally record the dose, unit, and form. These fields describe the caregiver's existing plan. | <img src="screenshots/add-medication.jpg" width="260" alt="Medication form with name, dose, unit, and drug form fields"> |
| **Repeat schedule**<br>Pick a start time and a repeat interval from one to 24 hours. The schedule can run indefinitely or stop after a set number of days. | <img src="screenshots/schedule.jpg" width="260" alt="Schedule form with frequency, starting time, AM and PM, and duration controls"> |
| **Instructions, notes, and review**<br>Add optional care instructions and personal notes. Check the schedule summary before saving. | <img src="screenshots/schedule-summary.jpg" width="260" alt="Care instructions, personal notes, and schedule summary above the Save medication button"> |
| **Local reminders and quick actions**<br>The phone schedules and delivers reminders, including when offline. On the iPhone shown here, the notification displays the medication and patient names and offers **Taken / Given** and **Remind me later**. Tapping the reminder opens the dose screen. Lock-screen visibility follows device preview settings. | <img src="screenshots/lock-screen-reminder.jpg" width="460" alt="iPhone notification with a medication title, patient name, and two reminder actions"> |
| **Snooze**<br>Postpone a dose reminder and see the updated reminder time on Today. The default delay can be changed in Settings. | <img src="screenshots/snoozed-dose.jpg" width="260" alt="Today screen showing a snoozed dose and its next reminder time"> |
| **Dose history**<br>Review the active patient's last seven days. The history distinguishes on-time, late, missed, and snoozed doses and summarizes on-time completion. | <img src="screenshots/history.jpg" width="260" alt="Seven-day medication history with on-time, late, and missed totals and recent events"> |
| **Sign-in and onboarding**<br>Create an account, verify an email address, or sign back in with email and password. CareDose remembers welcome completion on the device. Apple and Google sign-in depend on provider configuration and platform support. | <img src="screenshots/sign-in.jpg" width="260" alt="CareDose sign-in and sign-up tabs with email and password fields"> |

### Settings and privacy

- Set the appearance to System, Light, or Dark, and choose system, 12-hour, or 24-hour time display. Supported transitions respect Reduce Motion.
- Turn reminders and sound on or off, set the default snooze delay, and send a test reminder. Reminder preferences and operating-system permissions are set on each device.
- Manage account email and password, sign out, and review privacy and terms. Privacy controls cover consent, withdrawal, clearing local care data, and requesting account deletion.
- Choose whether to share optional diagnostics. Structured diagnostics can go to Sentry, and usage metrics can go to Supabase when configured. The diagnostic payloads exclude care records.
- Review a support email before sending it from the device's mail app.

## Supabase integration

CareDose uses **Supabase Auth** for identity and **Supabase Postgres** to sync care records between devices signed into the same account. The synced record contains patient profiles, medications and schedules, instructions and notes, and dose history. Local notification scheduling and delivery stay on each phone.

```mermaid
flowchart LR
    A["CareDose on device A"] -->|"Sign in"| Auth["Supabase Auth"]
    A -->|"Save edits first"| Local["Local care records and pending edits"]
    Local -->|"Retry when connected"| Cloud["Supabase Postgres<br/>Account-scoped care records"]
    Cloud -->|"Refresh account records"| A
    Cloud <-->|"Same account"| B["CareDose on device B"]
    Local -->|"Schedule on device"| Reminder["iOS or Android reminders"]
```

| Area | How it works |
| --- | --- |
| **Account isolation** | Each cloud record belongs to a Supabase user ID. Postgres row-level security scopes authenticated access to the current account. Anonymous access and client changes to record ownership are blocked. |
| **Offline edits** | The app saves changes locally first. It retries upload while open and checks again when brought to the foreground. A device receives another device's changes when it next syncs. |
| **Concurrent edits** | CareDose compares the last synced records, local edits, and current cloud records. It can merge edits to different records. Competing edits to the same record are preserved locally and flagged. The beta has no in-app conflict resolution yet. |
| **Consent and import** | The app asks for cloud sync consent. Uploading existing records already saved for that account on the device is a separate choice. |
| **Local storage** | Custom native builds use SQLCipher with a key kept in the device's secure storage. Expo Go and web are preview environments for fictional data and do not use the native encrypted database. |
| **Account deletion** | The backend is configured to remove the synced care record when the account is soft-deleted. The app also offers a separate local data clearing action. |

## Beta status

CareDose is built with React Native, Expo, Expo Router, and TypeScript. The screenshots show the iPhone beta, including a delivered local reminder and recorded dose outcomes. The Supabase schema is deployed and its access rules have been checked. Full two-device sync, an offline edit through an Expo Go restart and reconnection, and account deletion still need end-to-end verification.

Enterprise workspace features are marked **Coming soon**. Reminder delivery depends on device permissions and operating-system behavior. CareDose helps track an existing medication plan and does not replace instructions from a care team.
