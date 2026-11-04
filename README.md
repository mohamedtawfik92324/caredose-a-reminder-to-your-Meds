<h1 align="center"><strong>CareDose</strong></h1>

<p align="center"><strong>Medication reminders for you and the people you care for.</strong></p>

<p align="center">

![Build status](https://img.shields.io/badge/build-beta%20verification-yellow)
![License](https://img.shields.io/badge/license-MIT%20proposed-lightgrey)
![Tech stack](https://img.shields.io/badge/stack-Expo%20%7C%20React%20Native%20%7C%20TypeScript-3178C6)
[![Stars](https://img.shields.io/github/stars/mohamedtawfik92324/caredose-a-reminder-to-your-Meds?style=social)](https://github.com/mohamedtawfik92324/caredose-a-reminder-to-your-Meds/stargazers)

</p>

CareDose helps you turn an existing prescription schedule into daily reminders and see what still needs attention. Caregivers can keep a separate schedule and dose history for each person they support.

[Features](#-features) · [Screenshots](#-screenshots) · [Setup](#-getting-started) · [Architecture](#-architecture-and-project-structure)

---

## ✨ Features

- **Custom schedules:** Set a starting time, repeat interval, and treatment duration. Add optional dose, form, instructions, and notes.
- **Local notifications:** Receive reminders on the phone, including offline. Mark a dose **Taken / Given** or snooze it from the notification.
- **Daily dashboard:** See upcoming doses, snoozed reminders, and progress for the selected patient.
- **Dose history:** Review taken, late, missed, and snoozed doses in a seven-day view.
- **Caregiver profiles:** Manage separate schedules under one signed-in account and switch between patients.
- **Offline storage and cloud sync:** Save changes locally first. With consent, CareDose retries syncing patient profiles, medications, schedules, and dose history to Supabase when connected.
- **Display and reminder preferences:** Choose System, Light, or Dark appearance, set a clock format, and adjust reminder sound and default snooze delay.
- **Privacy controls:** Review or withdraw consent, choose whether to import existing device records, clear local data, or request account deletion.

## 🛠️ Tech stack

| Layer | Technology |
| --- | --- |
| Mobile framework | React Native, Expo, and TypeScript |
| Navigation | Expo Router |
| State | React Context and hooks |
| Native storage | Expo SQLite with SQLCipher in custom native builds, with SecureStore for the encryption key |
| Notifications | Expo Notifications using iOS and Android local scheduling |
| Backend | Supabase Auth and Postgres with row-level security |
| Optional diagnostics | Sentry and Supabase usage events |
| Checks | Node.js test runner, TypeScript, and Expo lint |

<!-- Swap any row if your implementation changes, e.g. Mobile framework: [React Native / Flutter / Android Native]. -->

## 📸 Screenshots

| Home dashboard | Add medication | Alert notification |
| :---: | :---: | :---: |
| <img src="screenshots/today.jpg" width="240" alt="Daily medication dashboard"> | <img src="screenshots/add-medication.jpg" width="240" alt="Add medication form"> | <img src="screenshots/lock-screen-reminder.jpg" width="240" alt="iPhone reminder with dose actions"> |
| Daily progress and scheduled doses | Medication details and repeat schedule | Lock-screen reminder and quick actions |

---

## 🚀 Getting started

Set `CAREDOSE_SOURCE_URL` to the Git URL of your private source checkout before running these commands. A documentation-only showcase does not contain a runnable app.

You’ll need Git, Node.js 22.13 or later, npm, and a Supabase test project. For Android, install Android Studio and configure an emulator. For iOS, use macOS with Xcode and an iOS Simulator.

```bash
git clone "$CAREDOSE_SOURCE_URL" caredose
cd caredose
npm ci
```

Create `.env.local` from `.env.example` and set `EXPO_PUBLIC_SUPABASE_URL` and `EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY` for your test project. Apply the project’s Supabase migrations before testing sync. Keep database passwords and service-role keys out of the client.

```bash
# Android emulator
npm run android

# iOS Simulator (macOS)
npm run ios

# Project checks
npm test
npm run typecheck
npm run lint
```

For native SQLCipher or OS-level reminder testing, use a custom development build. Expo Go is suitable for previewing with fictional records. Verify notification permissions, lock-screen actions, and offline delivery on a physical device.

## 🧩 Architecture and project structure

Supabase Auth identifies the signed-in account. CareDose saves edits on the device and syncs the account’s care records to Postgres under row-level security. Changes to different records can merge. Competing edits to the same record are flagged and the local edit is preserved. Each phone schedules its own reminders.

```text
caredose-a-reminder-to-your-Meds/
├── app/          # Expo Router screens and navigation
├── components/   # Shared UI and feature components
├── providers/    # Auth, care, medications, privacy, and preferences
├── lib/          # Services and utilities for storage, sync, and notifications
├── assets/       # App images and icons
├── supabase/     # Database migrations and account deletion function
└── tests/        # Regression checks
```

<!-- This project uses app/ and lib/ for the roles often assigned to src/, services/, and utils/. Update the tree if you reorganize the source. -->

## 🗺️ Roadmap

- Add a way to compare and resolve conflicting edits from two devices.
- Explore refill reminders and pharmacy integrations where supported.
- Evaluate Apple HealthKit and Android Health Connect integration.
- Research drug interaction alerts using a validated clinical data source and appropriate review.

These are future ideas. CareDose supports an existing medication plan and does not provide clinical advice.

---

## 🤝 Contributing and license

For a source repository that accepts contributions, open an issue to discuss substantial changes. Fork the repository, make a focused change, and open a pull request that explains the behavior and how you verified it. Use fictional records in tests and screenshots.

**Proposed license: MIT.** Confirm the license and add a `LICENSE` file before distributing the source under MIT terms.

<!-- Replace the build badge with your GitHub Actions workflow badge when CI is configured. Remove or update the license badge until the license is confirmed. -->

