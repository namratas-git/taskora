# Taskora

Taskora is a front-end productivity app prototype with three workspaces: **Personal** (private), **Shared** (family and friends) and **Work** (professional teams). It also has a calendar and real progress tracking. It has a warm, playful look, with a sun and moon that follow the time of day.

**Status:** Prototype / demo. It is not production-ready.

## Project Files and Deployment
Taskora is a static site: no server, no build step and no external files. Everything the app needs (code, React and fonts) is embedded in one HTML file.

```
taskora/               <- the folder you publish (repository root)
|-- index.html         <- the app; GitHub Pages opens this file
|-- standalone.html    <- the same app, kept as the stand-alone copy to open by double-click
|-- README.md
`-- .nojekyll          <- tells GitHub Pages to serve the files as they are
```

- `index.html` and `standalone.html` are byte-for-byte identical. `standalone.html` is the original; `index.html` is its copy under the name GitHub Pages requires. To change the app, replace `standalone.html` with the new version, then run `cp standalone.html index.html` and check with `cmp index.html standalone.html` (no output means they match) before you commit.
- Both files use no absolute paths, so they work from a project URL such as `https://<user>.github.io/<repository>/` and from a double-click on your computer.

### Publish on GitHub Pages
1. Create a new repository on GitHub (for example `taskora`) and upload everything in this folder, with `index.html` at the top level (not inside another folder).
2. In the repository open **Settings > Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**, choose the `main` branch and the `/ (root)` folder, then **Save**.
4. After a minute or two the site is live at `https://<your-username>.github.io/<repository>/`. Pages can cache for a few minutes, so use a hard refresh (Ctrl+Shift+R, or Cmd+Shift+R) after an update.

### Hosting notes
- Demo data is saved with the browser's local storage, which is shared by every site under the same `<user>.github.io` address. Taskora's keys all start with `taskora.`, so they do not clash with other projects. Each visitor has their own separate data in their own browser.
- Browser location (sun times) needs HTTPS and the visitor's permission. GitHub Pages provides HTTPS.

## Features

### Account Access (demo only)
**Authentication is for demonstration only.** There is no server and no real account. Everything runs in your browser, and nothing is sent anywhere. The sign-in page and the profile say so.

- **One fixed demo account:** `demo@taskora.app` with the password `taskora-demo` (shown on the sign-in page, and also one click on "Try the demo account"). The email is not case sensitive.
- **Sign in and sign out** work. Wrong emails and passwords are rejected with a clear message.
- **Session across refreshes:** staying signed in survives a page refresh. With Remember me ticked it also survives closing the tab or browser; without it, the session lasts for that browser tab.
- **Passwords are never stored.** The browser storage holds only the session (name, email, avatar colour, join date). The demo password is checked against a salted PBKDF2 hash (SHA-256, 100,000 iterations) that is part of the page code, and that hash is never written to storage either.
- **Your demo data is kept locally** and is not reset when you sign out or reload (see Data Saved in This Browser). A **Reset demo data** control on the Profile (two taps) puts everything back to the original samples.
- Sign-up, with name, email, password and confirmation, and the password-reset screen still exist. Accounts created this way live in memory only, so they are forgotten on reload and their tasks are not saved. Only the demo account is permanent.
- Form validation with clear error messages, and a one-click demo account.

### Home Dashboard
- Personal, Shared and Work workspace switch (Work uses its own `--work` accent in every theme).
- Today's task list with completion controls and a progress summary.
- Greeting and nudge that change with the time of day.
- Weekly progress bars based on your real tasks.
- New task button, available from the dashboard and the calendar.

### Work Workspace
A third tab for professional teamwork. Work data is kept separate from Personal and Shared: work tasks never appear in those tabs, and your personal tasks never appear in Work.

- **Workspaces (organisations):** four samples (Northwind Studio as Owner, Harbor Logistics as Manager, Greenway Community Group as Member, Bluebird Co-op as Viewer). Create your own with the New workspace sheet; you become its Owner. Switch between them with the chips.
- **Overview dashboard:** overall team progress, assigned-to-you, pending, overdue and completed counts, status breakdown, tasks needing attention, team progress and workload per member.
- **Tasks:** create tasks with title, project, priority (Low, Normal, High, Urgent), deadline, notes and one or more assignees. Status (To do, In progress, In review, Done), checklist, progress and filters by status, person and project. Two-step delete.
- **Projects and departments:** create projects (with a department) and view the tasks in each.
- **People and teams:** members with roles, create teams, add members to teams, change roles, remove members (two-step).
- **Invitations:** validated email and role, pending list, revoke, copy-message. See limitations.
- **Collaboration:** comments and shared updates on each task.
- **Roles and permissions** (a "Your access" card shows yours):

| Action | Owner | Admin | Manager | Member | Viewer |
|---|---|---|---|---|---|
| View workspace | Yes | Yes | Yes | Yes | Yes |
| Create tasks, comment | Yes | Yes | Yes | Yes | No |
| Edit own or assigned tasks | Yes | Yes | Yes | Yes | No |
| Edit any task, assign to others, delete tasks | Yes | Yes | Yes | No | No |
| Create teams and projects | Yes | Yes | Yes | No | No |
| Invite people and change roles | Yes | Yes | No | No | No |

  Role changes only apply to people ranked below you, and nobody can grant Owner.
- **Search, notifications and calendar** include work tasks, labelled with their workspace. Work tasks assigned to you count toward achievements and streaks.

### Calendar
- Uses your device's real local date, and rolls over at midnight while the app is open.
- Flip-style month calendar with previous, next and Today controls.
- Tasks appear on their due dates; selecting a day shows that day's tasks.

### Tasks
- Create tasks with a title, date, optional notes, and workspace (personal or a shared space).
- Shared tasks can be assigned to a space member.
- Tick tasks done and undo them.
- Task details sheet with free-text notes and an interactive checklist (add, tick and remove steps) with progress shown on the task.
- In a space where you are a Viewer, notes and checklists are read-only.

### Sun and Moon
- Sunrise and sunset are calculated on your device from your location.
- The sun and moon, sky colours and greeting follow your local time through day, dusk, night and dawn.
- Location from the browser (when permitted), a built-in city list, or an estimate from your time zone.
- Clear messages when location is denied, unavailable, blocked or unsupported.
- Preview slider to see the sky at any time of day.
- Handles polar day and night.
- A chosen location is saved in this browser.

### Search
- Search across tasks, notes, checklist items, people, pages and actions.
- Matches are highlighted. Arrow keys, Enter and Escape work.

### Notifications
- Bell with an unread badge and a notifications panel.
- Reminders generated from your tasks: overdue, due today and due tomorrow.
- A sunset nudge and a streak-at-risk nudge.
- Session events such as invitations saved and achievements unlocked.
- Mark all read and dismiss.
- Reminders update while the app is open.

### Shared Spaces and Invitations
- Three sample spaces (Home, Goa Trip, Capstone Group) with members, roles (Owner, Editor, Viewer) and progress.
- Space tasks, with assignees and your role shown on each space.
- Invite sheet for the space you own: email validation, role choice, and checks for your own address, existing members and duplicate invitations.
- Pending invitations list with role change, copy-message and revoke.
- Only a space's owner can invite; other roles see an explanation.

### Profile Menu
- Clickable avatar with a dropdown.
- Links to View Profile, Achievements, Streaks and Themes & Appearance (shows the active theme).
- Brightness slider (see Brightness) and Sign Out.
- Closes on outside click or Escape.

### Profile
- View name, email, avatar and join date.
- Edit name and avatar colour.
- Avatar palette of 12 colours, each with white-text contrast of at least 4.5:1. The ratio is shown on each swatch.
- Live preview of the avatar in the top bar and in a shared space while editing.
- Task statistics, achievements summary, and an Appearance card with Browse themes and Restore original.

### Themes & Appearance
- Dedicated page, opened from the profile menu or the Profile page.
- 14 themes in five groups:
  - Current theme: Taskora (the original look and the default).
  - Blues: Navy, Sky Blue, Slate Blue.
  - Greens: Sage, Teal, Forest Green.
  - Warm Accents: Terracotta, Vibrant Orange, Coral, Warm Yellow.
  - Deep Tones: Charcoal, Burgundy, Royal Purple.
- Colour swatches with theme names, and a clear Active badge.
- Live preview in day and night before applying.
- Applies across the app: backgrounds, cards, buttons, tabs, borders, highlights, icons and accents.
- Themes adapt to day, dusk, dawn and night. The sun and moon still follow your day.
- Text and accent colours in the 13 new themes meet WCAG AA contrast (4.5:1).
- Restore the original theme at any time.
- Saved in this browser, so it persists across reloads and sign-ins.

### Brightness
- Slider in the profile menu from 60% to 120%, with a Reset to 100% button.
- Applied as a screen overlay across the whole app.
- Saved in this browser.

### Productivity Statistics, Achievements and Streaks
Every number on the dashboard, Profile, Achievements and Streaks pages comes from one completion ledger, so they always agree and update the moment you tick a task. No statistic is hardcoded.

- **What counts:** a task counts when you complete it and it is yours: any Personal task, a Shared task assigned to you, or a Work task you are assigned to. Ticking someone else's task (for example as a Work Manager or Owner) does not count for you, and neither do tasks other members complete.
- **Each task once:** the ledger holds one entry per task. A task shared by several assignees counts once for you, and editing it, reassigning it or ticking it again cannot double count it.
- **Reopen and delete:** reopening a task removes its credit. Deleting a completed task removes it from your total. Incomplete tasks never count.
- **Tasks completed:** the total, with a small line under the profile tiles showing the split (Personal, Shared, Work).
- **Current streak:** consecutive days on which you finished at least one task in any workspace. Finishing nothing yet today keeps yesterday's streak alive until midnight; a full day without a task resets it to zero.
- **Best streak:** updated whenever you set a new record and never reduced, even if you later reopen or delete tasks.
- **Dates:** the device's local date and time zone are used, read at the moment you tick (so a task ticked just after midnight lands on the new day). Past completions keep the date they were made on.
- **Badges** (unlock permanently, with a progress bar while locked): First Step (1 task), Getting Started (10 tasks), Consistent (a 3-day streak), Task Champion (100 tasks) and Perfect Day (all of your tasks scheduled for one day, across Personal, Shared and Work, completed; needs two or more tasks that day). A toast and a notification appear when one unlocks.
- **Streaks page:** current and best streak, weekly activity strip, four-week heat map, and a two-step Clear history control (the only way to reset the best streak and badges).
- **Saved** in this browser per account (see below), so it survives reloads and sign-ins.
- **If the browser blocks storage** (a private window, or some embedded viewers), progress cannot be saved. Taskora detects this, shows a notice on the Profile, Achievements and Streaks pages and a toast on the first completion, and keeps progress in memory so it survives signing out and back in while the page stays open. It is lost on reload, and you are also signed out on reload. Use a normal browser window to keep it.
- Sample tasks that start out already done are sample data, not your activity, so they are not counted until you reopen and complete them yourself.

### Interface
- Responsive layout for desktop and mobile.
- Accessible labels, visible focus outlines and keyboard-friendly menus and sheets.
- Reduced-motion support.

## Data Saved in This Browser
Everything below is saved per browser, for the demo account only, and is never a password.
- Session: `taskora.demo.session` (tab session, or local storage with Remember me).
- Tasks, work organisations (members, teams, projects), and saved invitations: `taskora.demo.data.<email>`. Checked and trimmed when read back; if it is damaged the sample data is used.
- Profile (name, avatar colour, join date): `taskora.demo.profile.<email>`.
- Completion ledger (task ids, dates, best streak, unlocked badges; no titles): `taskora.demo.activity.<email>`.
- Preferences (brightness and theme): `taskora.prefs`.
- Chosen sun-times location: `taskora.demo.location` (cleared on sign-out).
- If the browser blocks storage, Taskora shows a notice, keeps your data in memory while the page stays open (including across sign-out and sign-in), and it is lost on reload.

## Current Limitations and Backend Requirements
- **Work (backend required):** invitations are only saved in the page. Email delivery, acceptance and adding invited people as members need a backend. Real multi-user collaboration, comments and updates reaching other people live, and assignment notifications need a backend and real-time service. Colleagues and their activity are sample data.
- Work workspaces, tasks, comments, teams and invitations are saved in this browser for the demo account only (see above).
- Not included in Work: ownership transfer, leaving or deleting a workspace, renaming or deleting projects, and editing a task title.
- The Admin role is implemented but you are never an Admin in the sample data, so it cannot be tried in the UI.
- Work progress: Done is 100%; otherwise checklist percentage; otherwise status weight (To do 0%, In progress 40%, In review 80%).- This is a front-end demo. It is not secure or production-ready, and the demo authentication must not be used with real passwords.
- **Demo authentication only:** anyone with the page can read the demo password and the hash in its source, there is no rate limiting or lockout, and there is no real identity check. It must never be used with real passwords or real personal data. A real login needs a backend identity service.
- Accounts are held in memory. A newly created account cannot be signed in to again after a reload, and its tasks are not saved. Only the built-in demo account is permanent.
- Saved data lives in this one browser. It does not sync across browsers or devices, and clearing the site's data (or using a private window) erases it. Another person using the same browser profile would see it. That needs a backend with per-user storage.
- Sample tasks are dated relative to the day you first signed in. Saved dates are kept as they are, so they become overdue as days pass; use Reset demo data to refresh them.
- Two tabs open at once each save their own copy, and the last one to save wins.
- The demo account's tasks, shared-space invitations and work organisations are saved in this browser. Notifications are rebuilt from your tasks each visit. The people in the sample spaces are sample data.
- Invitations are only saved in the page. Sending an email, accepting an invite and adding people to a space need a backend. The app generates a message to copy and says nothing is emailed.
- Real collaboration (shared tasks, assignments, roles, live updates between people) needs a backend. Other people's activity is sample data.
- In the sample spaces you are never a Viewer, so the Viewer read-only behaviour is implemented but cannot be tried in the demo.
- Reminders appear only while the app is open. Push and email notifications need a backend and a notification service.
- Password reset does not send an email.
- Changing your email address is not supported. It would need verification.
- Tasks cannot be edited after creation, and only Work tasks can be deleted. You can tick them, and edit notes and checklists.
- Statistics, streaks and badges are saved in this browser only. They do not sync across devices or browsers, and clearing site data erases them. Cross-device tracking needs a backend that stores completions per user account. Completions are kept even for tasks that no longer exist.
- Perfect Day is judged from the tasks in the current session.
- Theme and brightness are saved per device, not per account.
- Brightness is a screen overlay, not a hardware setting.
- City search uses a built-in list of about 90 cities, with no address lookup. Browser location may be blocked inside embedded viewers.
- Production use will require persistent storage, a real authentication service and an email service.
