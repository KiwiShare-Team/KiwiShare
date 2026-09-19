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

Post-fix Android screenshots or a short recording should be added after the
branch is deployed to staging. The local development machine does not have an
Android AVD; automated Flutter and API regression coverage is recorded in the
PR description.
