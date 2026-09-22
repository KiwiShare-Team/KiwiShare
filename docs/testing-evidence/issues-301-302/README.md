# QA evidence for issues #301 and #302

These screenshots capture the Android emulator failures reported by QA before
the fixes in PR #311.

## Issue #302: listing images

The Home screen rendered placeholders for valid product-image URLs.

![Home cards with missing images](before-home-image-loading.png)

## Issue #301: voice-message verification

The chat API rejected a recorded voice message because the deployed server
could not run its FFmpeg verification binary.

![Voice message verification error](before-voice-verification-error.png)

## Post-fix voice composer

The screenshot below is a deterministic 390 x 844 Flutter widget rendering of
the final recording state after tapping the microphone. It verifies that the
composer switches to the dedicated recording bar, exposes cancel and send
controls, and keeps the rest of the conversation layout stable.

![Voice recording composer after tapping the microphone](after-voice-tap-bar.png)

The widget snapshot is protected by the
`captures the voice recording composer for review evidence` regression test.
Flutter widget tests intentionally render text with their deterministic test
font, so the screenshot is evidence of layout and control state rather than a
device-font preview. The local development machine does not have an Android
AVD; automated Flutter and API regression coverage is recorded in the PR
description.
