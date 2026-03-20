# Plan: Build → Test on iPhone → Release on App Store
### Using Claude AI + GitHub + EAS Build (no Mac required)

---

## The Core Insight

You do not need a Mac to build and ship an iOS app in 2026.

The full pipeline looks like this:

```
You write code (Claude helps)
         ↓
    Push to GitHub
         ↓
  EAS Build compiles iOS app in the cloud
         ↓
  Automatically uploaded to TestFlight
         ↓
  You install it on your iPhone and test
         ↓
  Repeat until ready → submit to App Store
```

Every step is automated. You only need:
- A code editor (or Claude on your phone/browser)
- An iPhone
- An Apple Developer account ($99/year)
- A GitHub account (free)

---

## Prerequisites

| Item | Cost | Notes |
|------|------|-------|
| Apple Developer Program | $99/year | Required for TestFlight + App Store |
| GitHub account | Free | Where the code lives |
| Expo account | Free | EAS Build cloud infrastructure |
| iPhone (any) | Already have | For testing |

**Sign up for these before starting:**
1. [developer.apple.com/programs](https://developer.apple.com/programs) — enroll as Individual ($99/year)
2. [github.com](https://github.com) — create account if you don't have one
3. [expo.dev](https://expo.dev) — create free account

---

## Stack Decision

**React Native + Expo** is the only practical choice for this workflow.

| Why Expo wins here | Alternative (Swift/SwiftUI) |
|--------------------|----------------------------|
| No Mac or Xcode needed | Requires Mac + Xcode |
| EAS builds in the cloud | Must compile locally |
| Claude writes JS/TS (trained extensively on it) | Xcode setup is complex |
| Hot reload — see changes instantly | Build/run cycle is slower |
| One codebase = iOS + Android + Web | iOS only |
| EAS Submit → TestFlight in one command | Manual Xcode Archive process |

---

## Open-Source Starting Points

Rather than starting from zero, consider forking one of these:

| Repo | What it is | Best if you want to... |
|------|-----------|----------------------|
| [slopus/happy](https://github.com/slopus/happy) | Mobile + web client for Claude Code (React Native) | Build a Claude-powered app |
| [9cat/claude-code-app](https://github.com/9cat/claude-code-app) | Flutter Claude Code mobile app | Flutter alternative |
| [siteboon/claudecodeui](https://github.com/siteboon/claudecodeui) | Web UI for Claude Code sessions | Web-first approach |

If you're building something entirely different (not Claude-related), start fresh with `npx create-expo-app`.

---

## Phase 0 — Set Up Accounts & Tooling
**Goal:** Everything configured before writing a line of code.
**Time:** 1–2 days

### Step 1: Apple Developer Account

1. Go to [developer.apple.com/programs/enroll](https://developer.apple.com/programs/enroll)
2. Enroll as **Individual** ($99/year, paid annually)
3. Wait for approval — usually instant, sometimes up to 48 hours
4. Once approved, go to **App Store Connect** and create your app:
   - [appstoreconnect.apple.com](https://appstoreconnect.apple.com)
   - Apps → (+) New App → iOS → fill in name, bundle ID, SKU

### Step 2: App Store Connect API Key

This lets automated tools (EAS, GitHub Actions) act on your behalf without your password.

1. App Store Connect → Users and Access → Keys → (+) Generate API Key
2. Set role: **App Manager**
3. Download the `.p8` file **immediately** — you can only download it once
4. Note your: **Key ID**, **Issuer ID**, and the `.p8` file contents

Store these as GitHub repository secrets (covered in Phase 1, Step 3).

### Step 3: Install tools locally (or use GitHub Codespaces)

If you have any computer available (Windows, Mac, Linux):
```bash
# Install Node.js 20+ from nodejs.org, then:
npm install -g expo-cli eas-cli
eas login   # log in with your Expo account
```

> **No computer?** Use GitHub Codespaces — a full dev environment in your browser.
> Open your GitHub repo → Code → Codespaces → Create codespace.

---

## Phase 1 — Project Setup & Pipeline
**Goal:** First TestFlight build on your iPhone, zero code written yet.
**Time:** 1 day

### Step 1: Create the Expo project

```bash
# Start fresh:
npx create-expo-app@latest fanny --template blank-typescript

# OR fork an existing Claude iOS app, clone it, and cd into it
```

### Step 2: Configure EAS Build

```bash
cd fanny
eas init        # links project to your Expo account
eas build:configure  # creates eas.json
```

Your `eas.json` should look like this:

```json
{
  "cli": {
    "version": ">= 10.0.0"
  },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal",
      "ios": {
        "simulator": false
      }
    },
    "production": {
      "autoIncrement": true
    }
  },
  "submit": {
    "production": {
      "ios": {
        "appleId": "your@email.com",
        "ascAppId": "YOUR_APP_STORE_CONNECT_APP_ID",
        "appleTeamId": "YOUR_TEAM_ID"
      }
    }
  }
}
```

### Step 3: Add GitHub Actions secrets

In your GitHub repo → Settings → Secrets and variables → Actions → New repository secret:

| Secret name | Value |
|-------------|-------|
| `EXPO_TOKEN` | From expo.dev → Account Settings → Access Tokens |
| `ASC_API_KEY_CONTENT` | Contents of your `.p8` file |
| `ASC_API_KEY_ID` | Key ID from App Store Connect |
| `ASC_API_ISSUER_ID` | Issuer ID from App Store Connect |
| `ASC_APP_ID` | App ID from App Store Connect |

### Step 4: Add the GitHub Actions workflow

Create `.github/workflows/ios.yml`:

```yaml
name: iOS Build & TestFlight

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  build-and-submit:
    name: Build iOS + Upload to TestFlight
    runs-on: ubuntu-latest   # EAS builds in the cloud, so runner OS doesn't matter

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - uses: expo/expo-github-action@v8
        with:
          eas-version: latest
          token: ${{ secrets.EXPO_TOKEN }}

      - name: Build for iOS (production)
        run: eas build --platform ios --profile production --non-interactive

      - name: Submit to TestFlight
        run: eas submit --platform ios --profile production --non-interactive
        env:
          ASC_API_KEY_CONTENT: ${{ secrets.ASC_API_KEY_CONTENT }}
          ASC_API_KEY_ID: ${{ secrets.ASC_API_KEY_ID }}
          ASC_API_ISSUER_ID: ${{ secrets.ASC_API_ISSUER_ID }}
          ASC_APP_ID: ${{ secrets.ASC_APP_ID }}
```

### Step 5: Trigger your first build

```bash
git add .
git commit -m "chore: initial project setup"
git push origin main
```

Watch the Actions tab on GitHub. In ~15 minutes, a build appears in TestFlight on your iPhone.

**On your iPhone:**
1. Install the **TestFlight** app from the App Store
2. Accept the email invitation from Apple (App Store Connect will send one)
3. Your app appears in TestFlight — tap Install

---

## Phase 2 — Development Loop
**Goal:** Build features with Claude, test on iPhone, repeat.
**Time:** Ongoing

### The daily workflow

```
1. Describe what you want to Claude
2. Claude writes the code
3. git push → GitHub Actions runs automatically
4. ~15 min → new build in TestFlight
5. Open TestFlight on iPhone → Update → Test
6. Repeat
```

### Working with Claude effectively

Tell Claude:
- What the app does (be specific)
- What screen/feature you're building
- What you saw when you tested (exact behavior, screenshots)
- What you want to change

Example prompt:
> "I'm building a [describe your app] with React Native + Expo.
> I pushed a build and tested it on TestFlight. The login screen appears
> but after I enter my email and tap Next, nothing happens.
> Here's the relevant code: [paste it].
> Fix this and also add a loading spinner while the request is in flight."

### Project structure to maintain

```
fanny/
├── app/                    # Expo Router screens (file-based routing)
│   ├── (tabs)/
│   │   ├── index.tsx       # Home tab
│   │   └── profile.tsx     # Profile tab
│   ├── _layout.tsx         # Root layout
│   └── modal.tsx           # Modal screens
├── components/             # Reusable UI components
├── lib/                    # API clients, utilities
├── hooks/                  # Custom React hooks
├── constants/              # Colors, sizes, config
├── assets/                 # Images, fonts, icons
└── app.json                # Expo app config
```

### Accelerate testing: Development builds

For faster iteration (no 15-min wait), use a **development build**:

```bash
# Build once (takes ~15 min, then install on iPhone via TestFlight)
eas build --profile development --platform ios

# After that, start the dev server locally:
npx expo start --dev-client

# Your iPhone connects over the same WiFi — changes appear in seconds
```

---

## Phase 3 — App Store Submission
**Goal:** App live on the App Store.
**Time:** 1 week (Apple review takes 1–3 days)

### Checklist before submitting

**App Store Connect metadata** (fill these in at appstoreconnect.apple.com):

- [ ] App name (max 30 chars)
- [ ] Subtitle (max 30 chars)
- [ ] Description (max 4000 chars)
- [ ] Keywords (max 100 chars, comma-separated)
- [ ] Support URL
- [ ] Privacy Policy URL (required — must be a real URL)
- [ ] Category (primary + optional secondary)
- [ ] Age rating questionnaire completed
- [ ] Screenshots for iPhone 6.9" and 6.5" (required)
- [ ] App icon 1024×1024 PNG (no transparency, no rounded corners — Apple adds them)

**Technical checklist:**

- [ ] No placeholder content or "lorem ipsum"
- [ ] All features shown in screenshots actually work
- [ ] Crash-free on latest iOS version
- [ ] App does not request permissions it doesn't use
- [ ] Privacy policy covers all data collected
- [ ] If app uses a login, provide Apple reviewer credentials

**Privacy Policy:**
You must have one. Use a generator like [app-privacy-policy-generator](https://app-privacy-policy-generator.github.io/) and host it on GitHub Pages (free).

### Submit

The GitHub Actions workflow above submits production builds automatically.
Or manually:

```bash
eas build --platform ios --profile production
eas submit --platform ios --latest
```

### After submission

- Apple reviews within **1–3 business days** (usually 24 hours)
- You'll receive an email when approved or rejected
- If rejected, fix the specific issue they list and resubmit — the review queue resets but is usually fast on resubmission
- Once approved, you choose when to release (immediately or scheduled)

---

## Common Rejections & How to Avoid Them

| Rejection reason | How to avoid |
|-----------------|-------------|
| Missing privacy policy | Add a privacy policy URL before submitting |
| Crashes during review | Test on a real device, not just simulator |
| Misleading metadata | Screenshots must match actual app behavior |
| Requesting unnecessary permissions | Only request permissions you actually use |
| Incomplete app | Finish all features before submitting |
| Login required but no test credentials | Add reviewer account credentials in App Store Connect |
| Uses private APIs | EAS/Expo handles this — don't use any undocumented APIs |

---

## Architecture Decision for This Project

**Document in `docs/adr/0002-expo-react-native.md`** once this plan is adopted:

- **Decision:** React Native + Expo + EAS Build
- **Reason:** Only viable path to iOS without Mac; Claude generates TypeScript well; EAS handles all native complexity
- **Trade-off:** Not as performant as native Swift for complex animations; acceptable for most apps

---

## Quick Reference

| Task | Command |
|------|---------|
| Build for TestFlight (manual) | `eas build --platform ios --profile production` |
| Submit to TestFlight (manual) | `eas submit --platform ios --latest` |
| Development build (fast iteration) | `eas build --profile development --platform ios` |
| Start dev server | `npx expo start --dev-client` |
| Check build status | `eas build:list` |
| Update OTA (no review needed) | `eas update --branch production` |

> **OTA updates:** Expo's `eas update` lets you push JavaScript changes to existing users
> instantly, without going through App Store review. Use this for bug fixes and small changes.
> Only native code changes require a full App Store build.

---

## Summary Timeline

| Phase | What | Time |
|-------|------|------|
| 0 | Sign up for accounts, install tools | Day 1 |
| 1 | Project setup + pipeline + first TestFlight build on your iPhone | Day 2 |
| 2 | Build the app with Claude, test each push on iPhone | Weeks 2–N |
| 3 | App Store metadata, screenshots, submit | Final week |
| — | Apple review | 1–3 days |
| — | **App live on the App Store** | — |

---

*Created: 2026-03-20*
