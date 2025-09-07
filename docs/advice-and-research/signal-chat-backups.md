# Signal Chat Backups

The [Signal messaging app](https://signal.org/) is a great messaging app for secure messaging.

It provides a way to create backups of the signal chats that you have with others, through the _Chat backups_ feature.

## The issue with chat backups

These backups are only stored _locally_ (on the device) and backing up to a remote location (a file server or Cloud
storage solution) is not supported - see
the [Signal Support documentation](https://support.signal.org/hc/en-us/articles/360007059752-Backup-and-Restore-Messages).

This means that of your device gets lost, stolen, broken or corrupted, all your messages will be lost. For security,
this is quite decent, and means that no one will be able to get access to those messages... not even you! However, for
user experience, this kinda sucks.

If we want to save them, then we need to back up those backup files to a remote location such as a file server or cloud
storage solution - like Google Drive, Microsoft OneDrive or Dropbox.

## Requirements

Before digging into the solutions, it's helpful to understand the solution requirements:

- Work on Android
- Store chat backups in Google Drive (Android leads to Google products leads to Google Drive)
- Simple to set up the backup
- Ideally free, but I'd pay a small amount for an app to backups / syncs for me (not paying for a subscription though).
- Minimal or no advertisements (definitely no adverts if I pay for the app)
- Acceptable intrusions
	- Minimal or zero impact my battery life (don't need it always running, I just need a backup to be uploaded in the
	  night after the Chat backup is run).
	- Notification when the backup is done or failed (definitely if it failed).
	- Only asks for the permissions it needs (folder access on device, folder access in Google Drive) and nothing more.
	- Minimal or No gathering of data, and no sharing with 3rd parties (it's just a backup sync, I don't want to hand
	  over my personal data for that, especially if they're going to then share that data with 3rd parties).

## Backing up the backups

⚠️ At time of writing this is unsolved for me, in a way that is acceptable. The rest of this blog will instead be a
documentation of my findings and frustrations.

### Native Android feature?

Short answer: No.

Looking in `Settings > System > Backup` provides options for backing up Android things. The options are:

- Apps (no sub-options)
- Photos and videos (selecting opens Google Photos)
	- Backup quality
	- Mobile data usage
	- Back up device folders (only a limited list that doesn't include Signal, and would only back up photos & videos)
	- Back up Locked Folder
- SMS and MMS messages (no sub-options)
- Call history (no sub-options)
- Device settings (no sub-options)
- Google Account data (selecting opens a sub-menu)
	- List of Google apps for backup, none of which are Signal (obviously - it's not a Google app) or the `Files` app (
	  which is a Google app) for backing up files.

So, no ability to back up or sync device folders anywhere (not even to Google Drive).

### Files app feature?

I'm using the `Files by Google` app.

Short answer: No, but there is a useful `Back up to Google Drive` feature for backing up individual files manually.

There's no backup options in Settings.

If I go into `Internal Storage` and select my signal backup folder, there are no backup options but there are the
following relevant options:

- A `Copy to` option for copying files from one place to another, but not to a remote location.
- A `Quick Share` option but that's just for copying to nearby devices.

If I go into `Internal Storage`, and into the signal backup folder, and select my latest signal backup, there are the
following relevant options:

- A `Share` option that opens up the Android share menu - which could be used to manually upload the file to Google
  Drive, but nothing automatically at a certain time.
- A `Copy to` option for copying files from one place to another, but not to a remote location.
- A `Back up to Google Drive` option for backing up files - which could be used manually to back up the file to Google
  Drive, in a nice way that is in the "background" - but sadly does not provide a way to automatically do backups at a
  specific time each day.

### Google Drive feature?

Short answer: No.

In the menu there is an `Uploads` menu that just shows current uploads (nothing to be able to configure a regular
upload).

In `Settings` there is a `Backup and reset` option, but that just takes you to the Android backup settings, which proved
not helpful. There's no other relevant options in `Settings`.

Pressing the big <kbd>+</kbd> button allows us to create an Upload, but not a automatic regular upload for doing
automatic backups.

### A dedicated app for backups?

> Unix philosophy #1: Make each program do one thing well.

What if we had an app that just did backups really well. Looking at the Google Play Store, this is what's available:

- _Swift Backup_ by _SwiftApps.org_
- _Autosync_ (or other related apps) by _MetaCtrl_
- _My Backup Pro_ by _Rerware, LLC_
- _All Backup & Restore_ by _SuriDevs_

other backup apps seemed to either want to (a) backup to _their_ cloud storage and not Google Drive, or (b) backup
specific things like SMS messages or APKs (Apps).

#### Swift Backup

Short answer: No.

- ❌ Only allows for backing up apps, messages and call logs
- ℹ️ Free manual backups (of the above), requires payment for automatic backups - though it has a Lifetime plan for
  159kr.
- ⚠️ Requires "All files access" - bit much, could be using
  the [Storage Access Framework](https://developer.android.com/guide/topics/providers/document-provider) to only give
  access to specific files or directories.
	- (Incidentally, check which apps have this access: `Settings > Apps > Special app access > All files access`. It's
	  a bit annoying it's not available in the Permissions list for each App in Settings.)
- ✅ No account needed with Swift Backup service, which is nice.
- ✅ Good simple Android design.
- ✅ No sharing of data with 3rd parties.
- ⚠️ Some data collected (Personal Info, App info, and performance)
- ℹ️ Support for more advanced features, for phones that are rooted (which mine isn't).

#### Autosync

Short answer: Mostly yes, but requires payment for long-term use and requires some sacrifices on non-functional
requirements (security, data-sharing and battery usage).

I tried both _Autosync_ and _DriveSync_ (AutoSync for Google Drive):

- ℹ️ Trial-ware. Only get 14 days to test the app before requiring payment. Payment can either by a subscription (yuck)
  or a single-purchase lifetime license for a single storage provider (Google Drive, OneDrive, Dropbox) (better).
- ℹ️ Sync's folders between device and storage provider with different methods (two way, upload only, upload then
  delete, download only, etc.).
- ⚠️ Need to accept a EULA (license agreement) and privacy policy. It's fine, but a bit weird.
- ⚠️ Requires "All files access" - bit much, could be using
  the [Storage Access Framework](https://developer.android.com/guide/topics/providers/document-provider) to only give
  access to specific files or directories.
- ✅ No account needed with MetaCtrl or Autosync, which is nice.
- ✅ Simple app design.
- ✅ Provides good configuration options in Settings for autosync.
- ⚠️ Some data collected (Personal Info, App info, and performance)
- ❌ "Device or other IDs" are shared with third parties.
- ⚠️ Requires turning off battery optimization for the app to ensure it runs "reliably" (or... the developer could put
  the effort it to ensure reliability even under battery optimization conditions?)
- ❌ The "DriveSync" version brought up a large "Data preferences" screen after launch to select data gathering
  preferences for advertising purposes, etc. Many of the toggle options marked "Legitimate interest" (fuck off) were
  toggled on and required manually going through and toggling them all off, including diving into a "Vendor preferences"
  submenu (fuck off even more). The regular "Autosync" app did not ask for this, thankfully.

#### My Backup Pro

TODO

#### All Backup & Restore

TODO

### A dedicated app for automations?

Automation software allows the automation of lots of tasks, which could include backups. Looking at the Google Play
Store, this is what's available:

- _Automate_ by _LlamaLab_
- _IFTTT_ by _IFTTT, Inc_
- _Tasker_ by _joaomgcd_
- _MacroDroid_ by _ArloSoft_

On the Signal subreddit I also saw mentions of the `Syncthing`, but after searching it on the web, I
found [this article](https://forum.syncthing.net/t/discontinuing-syncthing-android/23002) detailing that the developer
is retiring Android support, so I'm not even going to dig into that as an option.

... still need to dig into this more and figure it out. Will report back.
