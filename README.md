<h1 align="center">CareDose</h1>

<p align="center"><strong>A calmer way to stay on top of medication.</strong></p>

<p align="center">
  <img src="https://img.shields.io/badge/React%20Native-61DAFB?logo=react&logoColor=black" alt="React Native">
  <img src="https://img.shields.io/badge/Expo-1C2024?logo=expo&logoColor=white" alt="Expo">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Supabase-3FCF8E?logo=supabase&logoColor=black" alt="Supabase">
  <img src="https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white" alt="SQLite">
</p>

<p align="center">
  A medication reminder app for individuals and family caregivers.<br>
  Built to make daily schedules easier to follow, one dose at a time.
</p>

---

## The idea

Managing medication becomes harder when there are multiple schedules and people to care for. CareDose brings those schedules into one clear daily view, with reminders and a history of what happened after each dose was due.

## A look inside

<table>
  <tr>
    <th>Today</th>
    <th>Add medication</th>
    <th>Reminder</th>
  </tr>
  <tr>
    <td align="center"><img src="screenshots/today.jpg" width="240" alt="Daily care dashboard showing medication progress"></td>
    <td align="center"><img src="screenshots/add-medication.jpg" width="240" alt="Medication details form"></td>
    <td align="center"><img src="screenshots/lock-screen-reminder.jpg" width="360" alt="iPhone medication notification with quick actions"></td>
  </tr>
  <tr>
    <td align="center">See the day’s doses and progress</td>
    <td align="center">Create a schedule around the care plan</td>
    <td align="center">Take action from a local notification</td>
  </tr>
</table>

## What it does

- **Daily overview** — see scheduled doses, progress, and the active patient’s care plan.
- **Medication schedules** — set a start time, repeat interval, and duration, with optional dosage and instructions.
- **Reminders** — receive local notifications and mark a dose taken or snooze it.
- **Caregiver profiles** — keep schedules for multiple people under one account.
- **Dose history** — review on-time, late, missed, and snoozed events.
- **Offline changes** — save care records locally and retry cloud sync when connected.
- **Personal preferences** — choose appearance, time format, sound, and snooze duration.

## How it’s built

**Expo Router** organizes the app into screens and navigation flows. **React Context** coordinates authentication, patients, medications, and preferences, while dedicated modules handle storage, schedules, and notifications.

Custom native builds store local care records with **SQLite and SQLCipher**. **Supabase Auth and Postgres** provide account identity and account-scoped sync through row-level security. Each device schedules its own notifications, so reminders can be delivered offline.

Changes to different records can merge during sync. When devices edit the same record, the app preserves the local change and flags the conflict. A conflict-resolution interface and full two-device verification remain open work.

## Project status

CareDose is a personal beta and portfolio case study. The screenshots show the iPhone beta. The Supabase schema is deployed, while cross-device sync and offline recovery still need end-to-end verification.

This repository contains the showcase and screenshots. The application source is private. The app is a reminder tool for an existing care plan and does not replace instructions from a care team.
