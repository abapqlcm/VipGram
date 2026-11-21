<div align="center">

# 👑 VipGram

**Telegram Modded Client for Android · Built for Power Users**

<br/>

![Android](https://img.shields.io/badge/Android-8.0%2B-3DDC84?style=flat-square&logo=android)
![Telegram](https://img.shields.io/badge/Telegram-12.10.6-26A5E4?style=flat-square&logo=telegram)
![Architecture](https://img.shields.io/badge/Arch-arm64--v8a-0A0A0A?style=flat-square)
![Build](https://img.shields.io/badge/Build-GitHub_Actions-2088FF?style=flat-square&logo=githubactions)
![Patches](https://img.shields.io/badge/Patches-8-D4AF37?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-0A0A0A?style=flat-square)

</div>

---

## Why VipGram Exists

You use Telegram daily. You've probably wished for:

- Messages that **don't disappear** when someone deletes them "for everyone"
- **No sponsored messages** cluttering your chats
- **Faster downloads/uploads** without hitting server limits
- **Screenshots in secret chats** without workarounds
- **View stories anonymously** — no "seen" receipt sent
- **Keep self-destruct media** forever, silently
- A **premium badge** that's purely cosmetic (local only)

VipGram delivers all of this — **client-side only**, no proxy servers, no third-party infrastructure, no account risk beyond your own API key.

---

## Quick Specs

| Property | Value |
|----------|-------|
| **Package** | `com.vipgram.rez` (installs alongside official Telegram) |
| **Base** | Telegram for Android 12.10.6 (DrKLO/Telegram) |
| **Architecture** | `arm64-v8a` only (~30 MB APK) |
| **Build** | GitHub Actions (fully automated, ~50 min) |
| **API Credentials** | Injected at build time via GitHub Secrets |
| **Developer** | [@iprez](https://t.me/iprez) |

---

## The 8 Patches

Each feature is a single boolean flag in `BuildVars.java`, maintained as one unified patch: `vipmod.patch`

| # | Feature | Flag | What It Does |
|---|---------|------|--------------|
| 1 | **Anti-Delete** | `NO_ADS_AND_KEEP_MESSAGES` | Ignores `updateDeleteMessages` / `updateDeleteChannelMessages` — deleted messages stay in *your* local DB |
| 2 | **Ad-Free** | `NO_ADS_AND_KEEP_MESSAGES` | Blocks `promoDialogId` + returns `null` for `getSponsoredMessages()` |
| 3 | **Turbo Download** | `SPEED_BOOST` | 12 parallel connections, 512 KB chunks (was 4×128 KB) |
| 4 | **Turbo Upload** | `SPEED_BOOST` | 2 MB chunks, capped at 16 parallel (fixed: was 64 KB → 256 conn → server throttling) |
| 5 | **Free Screenshots** | `NO_SCREENSHOT_RESTRICTION` | Removes `FLAG_SECURE` from secret chats, media viewer, secret media |
| 6 | **Keep Self-Destruct** | `KEEP_SELF_DESTRUCT_MEDIA` | `sendSecretMessageRead = null` → timer never starts, TTL not reported, save/forward buttons stay visible |
| 7 | **Local Premium Badge** | `LOCAL_PREMIUM_LOOK` | Sets `user.premium = true` only for your account — **zero server features unlocked** |
| 8 | **Ghost Stories** | `GHOST_STORY` | Views stories locally, never sends `TL_stories_incrementStoryViews` |

> **All patches are visible and auditable in `vipmod.patch`** — no hidden binary blobs.

---

## Branding

<div align="center">

| Asset | Preview |
|-------|---------|
| **App Name** | VipGram |
| **Icon** | <img src="https://raw.githubusercontent.com/abapqlcm/VipGram/main/.github/assets/logo.png" width="96" height="96" alt="VipGram Logo"> |
| **Vector** | [logo.svg](.github/assets/logo.svg) |
| **Raster** | [logo.png](.github/assets/logo.png) (1024×1024) |

</div>

Different package name = installs **side-by-side** with official Telegram. Two accounts, one phone.

---

## Build Pipeline

```
┌─────────────────────────────────────────────────────────────┐
│                    GITHUB ACTIONS (ubuntu-latest)           │
├─────────────────────────────────────────────────────────────┤
│  1. Checkout this repo (vipmod.patch)                       │
│  2. Clone DrKLO/Telegram + ALL submodules (recursive)       │
│  3. Apply vipmod.patch (git apply --check → git apply)      │
│  4. Inject API_ID / API_HASH from GitHub Secrets (sed)      │
│  5. Setup: JDK 21 (Temurin) + Android SDK 36 + NDK 27       │
│  6. Gradle: ./gradlew :TMessagesProj_App:assembleAfatRelease │
│       -Xmx6g -XX:MaxMetaspaceSize=1g                        │
│       org.gradle.parallel=true workers.max=2                │
│  7. Upload APK as Artifact (30 days) + GitHub Release       │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
                   SIGNED FAT APK (ready to install)
```

**Triggers:**
- Push to `main` (only when `vipmod.patch` or `.github/workflows/build.yml` changes)
- Manual: `workflow_dispatch` from Actions tab

---

## Install

1. Go to **[Actions](../../actions)** → latest green ✅ run
2. Download `VipGram-APK` artifact → unzip → install APK
3. Enable "Install unknown apps" if prompted
4. Sign in with your number — done

> First launch takes a few seconds (local DB initialization).

**Alternative:** Grab from **[Releases](../../releases)** if artifacts expired.

---

## For Developers

Want to add/remove/modify patches?

```bash
# 1. Clone this repo
git clone https://github.com/abapqlcm/VipGram.git
cd VipGram

# 2. Clone upstream + apply patch
git clone https://github.com/DrKLO/Telegram.git telegram
cd telegram
git submodule update --init --depth 1 --recursive
git apply ../vipmod.patch

# 3. Make your changes in TMessagesProj/src/main/java/...

# 4. Generate updated patch
git diff > ../vipmod.patch

# 5. Verify it applies cleanly
cd ../telegram && git apply --check ../vipmod.patch

# 6. Commit & push → CI builds automatically
git add vipmod.patch
git commit -m "mods: <describe change>"
git push origin main
```

**Constraints:**
- Upstream source ~3 GB, build needs 4–8 GB RAM → **don't build locally**, use CI
- If upstream changes break the patch → CI fails with `Patch failed to apply` → rebase `vipmod.patch`
- New flags must be declared in `BuildVars.java`

---

## Security Model

| Layer | Implementation |
|-------|----------------|
| **API Keys** | Never in code. Injected at build via GitHub Encrypted Secrets → masked in logs as `***` |
| **Network** | Direct MTProto to Telegram servers. No proxy, no middleware. |
| **Source** | 100% visible in `vipmod.patch` — fully auditable |
| **Signing** | Debug keystore (personal use). For distribution: generate your own keystore |

---

## Legal & Ethics

- **Personal project** — you assume all risk
- **Anti-delete / Ghost features** → other party won't know. Use with respect.
- **Your API key = your responsibility**. Spam/abuse traces to *your* account.
- **License:** MIT for this repo. Upstream (Telegram Android) is GPLv2 — binary redistribution must comply.

---

## FAQ

<details>
<summary><b>Will my account get banned?</b></summary>
VipGram uses the official protocol and source. Patches only alter *client-side* behavior. No guarantee — but risk is comparable to official client. Don't spam.
</details>

<details>
<summary><b>What happens when Telegram updates?</b></summary>
CI always pulls latest upstream. If patch conflicts → build fails → patch needs rebasing.
</details>

<details>
<summary><b>Does the premium badge unlock real Premium features?</b></summary>
**No.** It's purely visual. 4 GB uploads, premium stickers, translation, etc. are server-gated.
</details>

<details>
<summary><b>Why arm64 only?</b></summary>
Cuts APK size in half. Android 8+ (2017+) is all arm64.
</details>

<details>
<summary><b>How do I report a bug?</b></summary>
Open an [Issue](../../issues/new/choose). Include: steps to reproduce, device/Android version, logs if possible.
</details>

<details>
<summary><b>Can I build for iOS?</b></summary>
No. iOS codebase is completely separate.
</details>

---

## Repo Structure

```
VipGram/
├── vipmod.patch                    # All 8 patches (single file, auditable)
├── .github/
│   ├── workflows/
│   │   └── build.yml               # CI pipeline
│   └── assets/
│       ├── logo.svg                # Vector logo
│       └── logo.png                # 1024×1024 raster
├── README.md                       # This file
└── LICENSE                         # MIT
```

---

<div align="center">

---

**Built by [@iprez](https://t.me/iprez) · For personal use · MIT Licensed**

*If this saves you time, a ⭐ is appreciated*

</div>