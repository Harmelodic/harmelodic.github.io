# Fairphone OS - January 2025 - Bugs and Fixes

Fairphone released a new version of *Fairphone OS* on January 27th 2025:

- Fairphone 5: https://support.fairphone.com/hc/en-us/articles/18682800465169-Fairphone-5-OS-Release-Notes
- Fairphone 4: https://support.fairphone.com/hc/en-us/articles/4405858220945-Fairphone-4-OS-Release-Notes

The new version is: `B.098`

On my phone (a Fairphone 5), the build number is called: `FP5.UT2K.B.098.20250109`

I've seen two weird behaviours so far with it.

## NFC Disabled

Weird that it was turned off.

Easy fix: Re-enable NFC!

## "Device unlock" with fingerprint didn't work

Very weird one.

By going to `Settings > Security and privacy > Device unlock > Face and fingerprint unlock`, I could see that
`Unlock your phone` was enabled and that I had fingerprint(s) registered.

I could also see that the fingerprint sensor still worked, as I could use it within apps where I could log in to the
apps using a fingerprint.

Things I tried that didn't work:

- Rebooting the phone
- Disabling Face/Fingerprint `Unlock your phone`, rebooting and then re-enabling.
- Removing the registered fingerprints and re-registering them.

What ***did*** work was: Go to `Settings > Security and privacy > Device finders > Find My Device` and disabled
`Use Find My Device`. Thankfully, I found that re-enabling `Use Find My Device` afterwards did not re-break fingerprint
unlock.

Source of this finding
was [this reddit post](https://www.reddit.com/r/fairphone/comments/1ibhz4c/fingerprint_unlock_doesnt_work_with_latest_update/)
which led me
to [this Fairphone Forum thread/comment](https://forum.fairphone.com/t/software-update-fp5-ut2k-b-098-20250109/115532/17?u=furdiburd) -
so, thanks to user `UPPERCASE` for finding the fix.

---

That's all the bugs and fixes that I've seen for now.

~ Matt Smith / Harmelodic
