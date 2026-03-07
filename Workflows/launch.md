---
description: Pre-launch checklist for submitting an iOS app to the App Store
---

# Launch Workflow

Use this checklist before every App Store submission. Go through each section top-to-bottom.

## 1. Code Cleanup
- [ ] Remove all `print()` statements and debug logging from production code.
- [ ] Remove any `#if DEBUG` test data or mock overrides that shouldn't ship.
- [ ] Ensure no hardcoded API keys or secrets are in the source code (use `.xcconfig` or environment variables).
- [ ] Set the correct **build number** and **version string** in the target's General settings.

## 2. Functional Testing
- [ ] Test every core user flow end-to-end on a **real device** (not just simulator).
- [ ] Test on the **smallest supported device** (iPhone SE) and the **largest** (Pro Max).
- [ ] Test with **no network connection** — verify graceful error handling and offline states.
- [ ] Test with **slow network** (use Network Link Conditioner) — verify loading states appear.
- [ ] Test **dark mode** and **light mode** — verify all UI elements are visible.
- [ ] Test **Dynamic Type** at the largest accessibility size — verify layout doesn't break.

## 3. Assets & Appearance
- [ ] App icon is present in all required sizes (use a 1024x1024 source and let Xcode generate).
- [ ] Launch screen / splash screen is configured and looks correct.
- [ ] No placeholder images, lorem ipsum text, or TODO comments visible to the user.

## 4. Privacy & Legal
- [ ] Privacy policy URL is valid and accessible.
- [ ] App Tracking Transparency prompt is implemented if using IDFA or analytics that require it.
- [ ] Data collection disclosures in App Store Connect match actual app behavior (App Privacy section).
- [ ] Terms of service / EULA are provided if the app has subscriptions or user accounts.

## 5. App Store Connect
- [ ] App name, subtitle, and description are finalized.
- [ ] Keywords are filled in (30 characters max per keyword, 100 characters total).
- [ ] Screenshots are uploaded for all required device sizes.
- [ ] App preview video is uploaded (optional but recommended).
- [ ] Category and age rating are set correctly.
- [ ] Contact information and support URL are valid.

## 6. TestFlight (Recommended)
- [ ] Upload a build to TestFlight and run it through at least **one full beta cycle**.
- [ ] Collect and address feedback from beta testers before final submission.
- [ ] Verify in-app purchases and subscriptions work in the **sandbox environment**.

## 7. Final Submission
- [ ] Archive the app in Xcode (Product → Archive).
- [ ] Validate the archive (no errors or warnings).
- [ ] Upload to App Store Connect via Xcode Organizer.
- [ ] Submit for review with any necessary review notes (e.g., demo account credentials).
