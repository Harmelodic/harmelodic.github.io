# Principles for installed client apps

Just some... not finished or fully thought through yet.

"Installed client apps" mean things like mobile apps & desktop apps that provide some functionality and usually act as a
client accessing some remote (web) service or services.

## User Experience / Design

### Don't get in the way of what the user wants to do. When you must, do it at the last possible moment

When the app boots, don't have popups showing "what's new" or "welcome back" or even onboarding compliance steps or
requests for permissions. These are getting in the way of the user - especially if they just want to change something
like the language used or the theme to use, or want to explore how the app looks / functions without creating an
account. Instead:

- Provide "what's new" or "welcome back" via some notifications / updates screen, and try to draw the
  user's attention to this with something like a notification dot.
- Require "onboarding" steps or requests for permissions / access when the user tries to access the specific feature
  that requires the onboarding / access (e.g. becoming a customer / creating an account / doing a specific action).

This often means providing access to navigate the app for users _without_ an account for authentication &
authorization (for client apps for services that require this), as well as providing access to app features in an
"offline" mode, where cached data and offline features are available.

### Design for ergonomics & accessibility

To make using the app easy, put user controls closer to where the user will interact with the client, and information
away from the controls - e.g. on a phone, the controls should be at the bottom (near fingers) and information is at the
top (not hidden underneath fingers)

Ensure compatibility with accessibility tools for those with disabilities (e.g. screen readers for those with visual
impairments, or configuration options for easier visual / touch feedback for those with hearing impairments).

Ensure colour schemes are available for those with colour blindness, or only use colour schemes that work for all users.

### Don't hide functionality if it's not available, but keep separate (and partially hide) paid-for features

When a user is using the app, features might not be available to them to use because the feature requires certain
conditions (that don't involve payment) to be available, or it might be paid-for feature.

When the feature requires non-payment conditions to be met (e.g. to be online, authentication, etc.), then it is good to
keep the regular layout of the app, but grey out / fade (or de-highlight in some other way) and disable the feature and
provide informational text as to why the feature is unavailable. If the condition requires a user action to be met,
provide a button (or something) for the user to be able to complete the action and activate the feature. This is
because:

- Keeping the regular layout of the app means users can always be familiar with navigating in the app (and features are
  not hidden thus resulting in an unfamiliar layout that becomes harder to intuitively navigate).
- By greying out (or other de-highlight) and disabling, we effectively communicate to user that the feature _is_
  available in the app's offering, but is just _currently_ unavailable / disabled.
- By providing informational text up front (not requiring interaction) it informs the user of the reason for the feature
  being unavailable.
- By providing a button (or something else) for enabling features that require user action (e.g. authentication) it
  empowers the user to be able to enable the feature.

When the feature requires payment, is better to isolate the feature from the other features available on the same
screen, even hiding it, and inform the user that "premium features" require payment, with a button to take them to the
flow where payment can be offered. This is because:

- Keeping paid-for features separate mitigates some of the frustration gained by non-paying / lower tier users,
  resulting in a less hostile relationship between service provider and user.
- Keeping paid-for features visible but partially hidden (e.g. wrapped in
  an [accordion](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/)) allows you to showcase the value and relevance of
  paid-for features to users, and highlight that the features _is_ available but requires payment.

### Commit to a custom design, or go default - but stay accessible

If you have the money, time and resources to create a custom design (colour scheme and/or design system), then go for
it! Spotify wouldn't be iconically spotify if it was not black and green.

However, if you don't have (one or more of) the money, time or resources, then stick to the default styling of the
system and hook into the user's selected preferences for light/dark/system theming and colour schemes:

- Android = Material Design, including "Material You" user preferences.
- Apple (iOS / iPadOS / macOS) = Liquid Glass, Fluent for Windows, etc.)
- Windows = Fluent Design
- Linux = Gnome Human Interface Guidelines with GTK.

By using default components and the native design system, we gain the following benefits:

- The app will be easier to create and design
- The app will look modern and relevant for the system is running on.
- The app will fit more seamlessly into the operating system's design, resulting in the user gaining a more "native"
  experience rather than a custom one.
- Default theming of a design system has usually had accessibility already thought-through and catered for, so we know
  we can create an accessible app without much effort (this is not always the case though, and sometimes still
  requires extra considerations and development effort).
- The app will automatically be updated to stay modern when backwards-compatible design system changes occur (e.g.
  spacing, colour palette generation, etc.).
- For non-backwards compatible design system changes occur, the design system developers usually produce migration
  guides to make it easier to update the app code to adopt the new design system changes.

Where a default design system is not available (e.g. an Electron-based desktop app), then use an existing design system
that is maintained and supports your app platform. Material Design is often the go-to option for this.

## Understanding app usage

Understanding how your app is being used, and what users often do, like doing and want is very useful when developing
new features and refining the app.

### Feedback

Getting user feedback is one of the most important ways to achieve this. The app should provide a mechanism for the user
to easily provide feedback (both anonymously and not).

This should probably ultimately be fed into an "issue tracking" system for tracking user-driven bug reports and feature
requests - which should be made public for transparency with users. This should be separate from a developers internal
issue tracking / project management system.

### Telemetry

Gathering telemetry (in the forms of tracking data and performance data) is another way to gather information about app
usage. It is not a replacement for direct user feedback, but the data can provide insight into app usage even for users
who don't provide feedback.

#### Telemetry privacy

Gathering telemetry should be an _**opt-in**_ feature for users. Enabling and disabling telemetry (and different kinds
of telemetry) should be configurable in the app's settings menu.

Telemetry should ideally be completely anonymous to protect user privacy and remove applicability of GDPR to the data.
However, the nature of value of telemetry often requires some kind of personal data to be gathered that could identify a
user or be paired with other data points to ultimately identify a user. These identification data points are things
like (but not exhaustively):

- Personally identifiable data of the user (name, address, age, etc.)
- App installation ID
- Geolocation at time of event
- System information
- Timestamp of event (when paired with other timestamps of other events can trace behaviour, and thus identify a user)

Good to remember that "personal data" is not the same as "identity data", but simply any data that is related to a
specific person - i.e. it is data that is _personal_ to them. Consult legal experts when dealing with things like GDPR
Personal Data, or the USA's regulation for PII - different jurisdictions use different terms and some have broader or
narrow definitions.

Specific device and app installation information is useful to understand what sort of compatibility you should be
catering for. Other identifiable information could be processed to be made more vague / generic to aid in reducing
infringement of users privacy, but also to reduce cardinality of data, when stored and being queried.

#### Telemetry Architecture

Telemetry should be event driven, where actions performed by the user results in telemetry events produced.

To gather the telemetry data and ensure no loss of data, and respecting user internet bandwidth, telemetry events should
be stored offline (on device), and periodically uploaded for collection (i.e. the Outbox pattern). If the user has opted
out of telemetry, the events are deleted instead of uploaded. This allows for easy telemetry instrumentation of the app,
whilst decoupling whether that telemetry is actually collected or not to a separate part of the app.

In order to collect device or app installation information, a telemetry event created whenever the user opens the app,
or does some kind of regular action (like logging in). This can be more bandwidth / storage efficient than sending the
device or app installation information with every event. Other events can then just contain specific device and app
installation information that relevant to that telemetry event.

Name telemetry events as things that have happened, ideally connected to user flows and app screens. Things like
`user_logged_in`, `user_opened_account_overview`, `user_shared_post`.

## Service Connectivity

- When calling an API, have a way to communicate that the feature is no longer available, for when it won't be.
- For pull info: Inbox pattern, with local DB. Nothing direct.
- For pushing info: Outbox pattern, with local DB. Nothing direct.
- Never prevent or slow the user from using features that should work when offline -
  see [above](#dont-get-in-the-way-of-what-the-user-wants-to-do-when-you-must-do-it-at-the-last-possible-moment)
- If the user has saved lists (watch lists, notes, etc.) then save the content of the lists locally and sync them to
  servers - conflicts resolved by timestamp or version and attempt to merge.
- Show an offline/online/connecting status somewhere in the client.

## Settings

The settings should be organised very intuitively - it should be one of _the most_ intuitive parts of the client.

If the client shows connections between services & other clients (e.g. Spotify showing Now Playing on another device)
should not prevent the current client from being configured.

## Internationalisation and Localisation

a.k.a. `i18n` and `l10n`.

I quite like [the Wikipedia definition](https://en.wikipedia.org/wiki/Internationalization_and_localization) of these
terms, which is:

> Internationalisation is the process of designing a software application so that it can be adapted to various languages
> and regions without engineering changes.  
> Localisation is the process of adapting internationalised software for a specific region or language by translating
> text and adding locale-specific components.

Obviously, there will be processes and/or compliance things to account for when dealing with different countries or
regions, but the primary focus for client apps in regarding Locale is _language support_.

> Locale _can_ be used for some location-related business logic, but it might not be good enough for some regulations /
> compliance, given a user should be able to change their locale. Instead, determining the actual location of the user
> would need to be done (e.g. Using location data like IP geolocation or GPS), though many things can still be avoided
> by users (via VPNs and Tor) and turning of location services. Given the user-selectable nature of Locale, I would
> **not recommend** using it for business-logic and instead require actual location data, and leave Locale for language.
> This also aligns nicely with giving things a single purpose (a la _Unix philosophy_: Do one thing and do it well).

Handle localisation through some platform (e.g. [Weblate](https://hosted.weblate.org/)) for development, and then ship
languages with the application, to be handled completely offline:

- Each localisation should then come as a reference file (or files), let's call that a "localisation package".
- At install time, if possible, give option to install different localisation packages (install all, allow for opt-out).
- If not possible to allow the user to select at install time (e.g. mobile app), then install all localisation packages
  and then have the user select their locale / language as the first thing to do.
	- Default to the system locale when prompting the user, giving a simple OK to continue to use that locale, or
	  allow them to change the locale (either directly, or take them to the correct place in settings).
	- If system locale is not supported by your app, default to English (either American or British) or if the app has
	  an intended regional userbase, then the locale for that region (e.g. an Italian transport app could default to
	  `it-IT` for "Italian in Italy").

Use standard Locale codes (e.g. `en-GB`) made up of a [ISO 639](https://en.wikipedia.org/wiki/ISO_639) language
code (e.g. `en`) and a [ISO 3166](https://en.wikipedia.org/wiki/ISO_3166) country code (e.g. `GB`). This allows
for language differences between countries (e.g. American English vs British English, Swedish vs Finland Swedish) and
works nicely with the existing other standard things (e.g. `Accept-Language` HTTP header).
