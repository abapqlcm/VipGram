<div align="center">

# 👑 VipGram

### <sup>تلگرام شخصی‌سازی‌شده برای اندروید — بدون تبلیغ، بدون حذف، بدون محدودیت</sup>

<br/>

![Platform](https://img.shields.io/badge/platform-Android_8.0%2B-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Telegram](https://img.shields.io/badge/based%20on-Telegram_12.10.6-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)
![Arch](https://img.shields.io/badge/arch-arm64--v8a-0A0A0A?style=for-the-badge)
![CI](https://img.shields.io/badge/build-GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Mods](https://img.shields.io/badge/patches-8-D4AF37?style=for-the-badge)
![Developer](https://img.shields.io/badge/developer-@iprez-FFD700?style=for-the-badge&logo=telegram&logoColor=000)
![License](https://img.shields.io/badge/license-MIT-0A0A0A?style=for-the-badge)
![Status](https://img.shields.io/badge/status-active-3DDC84?style=for-the-badge)
![Language](https://img.shields.io/badge/language-Kotlin%2FJava-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)
![Size](https://img.shields.io/badge/APK-%3C_30MB-0A0A0A?style=for-the-badge)

</div>

---

## 🎯 نمای کلی

> **VipGram** یک کلاینت تلگرام **کاملاً شخصی** است که همه‌ی تغییراتش فقط در **سمت کلاینت** (اپ شما) اعمال می‌شود — بدون هیچ سرور واسطه، بدون تغییر در زیرساخت تلگرام.  
> کنار تلگرام اصلی روی **همون گوشی** نصب می‌شود و تداخلی ندارد.

<div align="center">

| | | |
|:---:|:---:|:---:|
| 📦 **Package** | `com.vipgram.rez` | نصب کنار تلگرام رسمی |
| 🏗️ **معماری** | `arm64-v8a` | APK سبک، بالا از Android 8 |
| 🔧 **بیلد** | GitHub Actions | ~۵۰ دقیقه، کاملاً خودکار |
| 🔑 **API** | Secrets | هرگز در کد هاردکد نیست |
| 📜 **مجوز** | MIT | کد باز، آدیت‌پذیر |

</div>

---

## 📑 فهرست

<details open>
<summary><b>📋 محتوای این سند</b></summary>

- [✨ قابلیت‌ها](#-قابلیت‌ها)
- [🎨 برندینگ و لوگوی اپ](#-برندینگ-و-لوگوی-اپ)
- [🏗️ معماری و بیلد](#-معماری-و-بیلد)
- [📥 نصب](#-نصب)
- [🛠️ توسعه و تغییر مودها](#-توسعه--تغییر-مودها)
- [🔐 امنیت](#-امنیت)
- [⚠️ نکات قانونی و اخلاقی](#-نکات-قانونی--اخلاقی)
- [❓ سؤالات متداول](#-سؤالات-متداول)
- [📂 ساختار ریپو](#-ساختار-ریپو)

</details>

---

## ✨ قابلیت‌ها

همه‌ی مودها با فلگ‌های `boolean` در `BuildVars.java` کنترل می‌شوند و به‌صورت **تک‌پچ** در `vipmod.patch` نگهداری می‌شوند.

### 📊 ماتریس قابلیت‌ها

| # | آیکون | قابلیت | فلگ BuildVars | وضعیت | تأثیر |
|:---:|:---:|---|:---:|:---:|---|
| 1 | 🛡️ | **ضدحذف پیام** | `NO_ADS_AND_KEEP_MESSAGES` | ✅ فعال | پیام‌های حذف‌شده «برای همه» نزد شما باقی می‌مانند (چت + کانال) |
| 2 | 🚫 | **حذف تبلیغات** | `NO_ADS_AND_KEEP_MESSAGES` | ✅ فعال | حذف بنر اسپانسر و پیام‌های PSA |
| 3 | ⚡ | **دانلود سریع‌تر** | `SPEED_BOOST` | ✅ فعال | ۱۲ درخواست موازی + چانک ۵۱۲KB |
| 4 | 🚀 | **آپلود سریع‌تر** | `SPEED_BOOST` | ✅ فعال | چانک ۲MB + سقف ۱۶ درخواست موازی |
| 5 | 📸 | **اسکرین‌شات آزاد** | `NO_SCREENSHOT_RESTRICTION` | ✅ فعال | برداشتن `FLAG_SECURE` از چت مخفی/ویوور |
| 6 | ♾️ | **مدیای تایمردار همیشگی** | `KEEP_SELF_DESTRUCT_MEDIA` | ✅ فعال | Self-destruct باز می‌ماند، ذخیره/فوروارد فعال |
| 7 | ⭐ | **نشان پرمیوم** | `LOCAL_PREMIUM_LOOK` | ✅ فعال | ستاره پرمیوم ظاهری، کاملاً محلی (بدون قابلیت سروری) |
| 8 | 👻 | **گوست استوری** | `GHOST_STORY` | ✅ فعال | دیدن استوری بدون اطلاع طرف مقابل |

<details>
<summary><b>🔍 جزئیات فنی هر مود</b> (باز کردن)</summary>

<br/>

| قابلیت | مکانیزم | نکات کلیدی |
|---|---|---|
| **🛡️ ضدحذف** | نادیده‌گیری `updateDeleteMessages` / `updateDeleteChannelMessages` | پیام فقط در دیتابیس **لوکال شما** می‌ماند |
| **🚫 ضدتبلیغ** | دو قطع: `promoDialogId` ست نمی‌شود + `getSponsoredMessages()` → `null` | هیچ بنر/پیام تبلیغاتی نمایش داده نمی‌شود |
| **⚡🚀 بوست سرعت** | `FileLoadOperation` / `FileUploadOperation` | فیکس شده: قبلاً چانک ۶۴KB → ۲۵۶ درخواست موازی → flow-control سرور. الان `Math.min(16, ...)` + چانک ۲MB |
| **📸 اسکرین‌شات** | `FLAG_SECURE` غیرفعال در `ChatActivity`، `PhotoViewer`، `SecretMediaViewer` | اسکرین‌شت/رکورد در چت مخفی و مدیای سکرت |
| **♾️ مدیای تایمردار** | `sendSecretMessageRead` = `null` → تایمر استارت نمی‌شود | TTL به سرور گزارش نمی‌شود، `galleryButton` مخفی نمی‌شود |
| **⭐ پرمیوم لوکال** | در `putUser` فقط برای اکانت خودتان: `user.premium = true` | **هیچ** قابلیت واقعی پرمیوم (آپلود ۴GB، استیکر،...) باز نمی‌شود |
| **👻 گوست استوری** | `TL_stories_incrementStoryViews` ارسال نمی‌شود | علامت‌گذاری seen فقط محلی |

</details>

---

## 🎨 برندینگ و لوگوی اپ

<div align="center">

| مورد | مقدار |
|:---:|---|
| 📛 **نام اپ** | **VipGram** |
| 📦 **Package** | `com.vipgram.rez` |
| 🏗️ **معماری** | `arm64-v8a` (APK سبک‌تر) |
| 👨‍💻 **توسعه‌دهنده** | [@iprez](https://t.me/iprez) |

<br/>

<img src="https://raw.githubusercontent.com/abapqlcm/VipGram/main/.github/assets/logo.png" alt="VipGram Logo" width="280" height="280">

<br/>
<sub>لوگوی رسمی VipGram — <a href=".github/assets/logo.svg">SVG وکتور</a> و <a href=".github/assets/logo.png">PNG</a> در <code>.github/assets/</code></sub>

</div>

> 💡 چون **package name** متفاوته، VipGram **کنار** تلگرام رسمی قابل نصبه و دو حساب همزمان می‌تونن فعال باشن.

---

## 🏗️ معماری و بیلد

بیلد **کاملاً خودکار** روی GitHub Actions انجام می‌شود — هیچ نیازی به کامپیوتر قوی ندارید.

### 🔄 جریان بیلد (CI Pipeline)

```mermaid
flowchart TD
    A[📥 Checkout Repo<br/>(vipmod.patch)] --> B[📦 Clone Telegram Source<br/>+ All Submodules]
    B --> C[🔧 Apply vipmod.patch<br/>(8 Mods)]
    C --> D[🔐 Inject API_ID / API_HASH<br/>(GitHub Secrets)]
    D --> E[☕ Setup JDK 21 + Android SDK + NDK]
    E --> F[⚙️ Gradle Build<br/>assembleAfatRelease]
    F --> G[📦 Collect APKs]
    G --> H[📤 Upload Artifact]
    G --> I[🚀 Publish GitHub Release]
    
    style A fill:#1a1a2e,stroke:#26A5E4,color:#fff
    style B fill:#1a1a2e,stroke:#3DDC84,color:#fff
    style C fill:#1a1a2e,stroke:#D4AF37,color:#fff
    style D fill:#1a1a2e,stroke:#FF6B6B,color:#fff
    style E fill:#1a1a2e,stroke:#7F52FF,color:#fff
    style F fill:#1a1a2e,stroke:#FFD700,color:#000
    style G fill:#1a1a2e,stroke:#2088FF,color:#fff
    style H fill:#1a1a2e,stroke:#3DDC84,color:#fff
    style I fill:#1a1a2e,stroke:#FFD700,color:#000
```

<details>
<summary><b>نسخه متنی (fallback)</b></summary>

```text
   ┌──────────────────────────────────────────────────────────────┐
   │                    🤖  GitHub Actions Runner                 │
   │                    ubuntu-latest · JDK 21 · NDK              │
   ├──────────────────────────────────────────────────────────────┤
   │                                                              │
   │   1️⃣  Checkout this repo (vipmod.patch lives here)           │
   │                                                              │
   │   2️⃣  Clone latest official Telegram source                  │
   │       + ALL submodules (recursive)                           │
   │                                                              │
   │   3️⃣  Apply vipmod.patch (8 mods)                            │
   │       → git apply --check → git apply                        │
   │                                                              │
   │   4️⃣  Inject API_ID / API_HASH from Secrets                 │
   │       → sed injection into BuildVars.java                    │
   │                                                              │
   │   5️⃣  Setup: JDK 21 (Temurin) + Android SDK 36 + NDK 27     │
   │                                                              │
   │   6️⃣  Gradle Build                                            │
   │       → -Xmx6g -XX:MaxMetaspaceSize=1g                      │
   │       → org.gradle.parallel=true                             │
   │       → ./gradlew :TMessagesProj_App:assembleAfatRelease    │
   │       --no-daemon --stacktrace                               │
   │                                                              │
   │   7️⃣  Collect APKs → Upload Artifact (30 days)              │
   │                                                              │
   │   8️⃣  Publish as GitHub Release (auto-tagged)               │
   │                                                              │
   └──────────────────────────────┬───────────────────────────────┘
                                  ▼
                      📦  VipGram-APK (Signed, Ready to Install)
```

</details>

### ⏱️ مشخصات بیلد

| مورد | مقدار |
|:---:|:---:|
| **زمان بیلد** | ~۵۰ دقیقه |
| **سنگین‌ترین مرحله** | کامپایل نیتیو C++ |
| **مصرف رم** | بهینه شده برای ۷GB (Xmx6g) |
| **پارالللیزم** | `workers.max=2` + `org.gradle.parallel=true` |
| **خروجی** | APK امضاشده (Fat APK) |

### 🔀 تریگرها

| رویداد | شرط |
|:---:|---|
| `push` به `main` | فقط وقتی `vipmod.patch` یا `build.yml` تغییر کرده باشند |
| `workflow_dispatch` | اجرای دستی از تب **Actions** |

---

## 📥 نصب

### 🎯 روش سریع (پیشنهادی)

1. به تب **[Actions](../../actions)** ریپو برو → آخرین بیلد سبز ✅
2. از بخش **Artifacts** فایل `VipGram-APK` رو دانلود کن
3. ZIP رو استخراج کن → APK رو نصب کن  
   *(اجازه‌ی *منابع ناشناس* در تنظیمات اندروید لازم می‌شود)*
4. با شماره‌ی خودت لاگین کن — تمام! 😎

> 🍃 اولین اجرا ممکنه چند ثانیه طول بکشه (ساخت دیتابیس محلی).

### 📱 روش جایگزین (Release)

اگر Artifact در دسترس نباشه، از تب **[Releases](../../releases)** آخرین نسخه رو دانلود کن.

---

## 🛠️ توسعه و تغییر مودها

### 🔧 پیش‌نیازها

- Git + Java 21 (برای بیلد لوکال، غیرپیشنهادی به خاطر رم)
- Android SDK + NDK (اگه لوکال بیلد می‌خوای)
- **پیشنهاد:** کل بیلد رو روی GitHub Actions انجام بده — رایگان، سریع، بدون دردسر

### 📋 مراحل توسعه

```bash
# 1. کلون ریپو VipGram
git clone https://github.com/abapqlcm/VipGram.git
cd VipGram

# 2. کلون سورس رسمی تلگرام + اعمال پچ
git clone https://github.com/DrKLO/Telegram.git telegram
cd telegram
git submodule update --init --depth 1 --recursive
git apply ../vipmod.patch

# 3. تغییرات دلخواه رو اعمال کن
# ویرایش فایل‌های جاوا/کاتلین در TMessagesProj/src/main/java/...

# 4. پچ جدید بساز
git diff > ../vipmod.patch

# 5. تست لوکال (اختیاری، نیازمند 8GB+ RAM)
# ./gradlew :TMessagesProj_App:assembleAfatRelease

# 6. کامیت و پوش → بیلد خودکار آغاز می‌شود
git add vipmod.patch
git commit -m "mods: update patch - <توضیح تغییر>"
git push origin main
```

### ⚠️ نکات مهم توسعه

| نکته | توضیح |
|:---:|---|
| 📏 **سورس تلگرام** | ~۳GB (شامل submodules) |
| 💾 **بیلد لوکال** | ۴-۸GB RAM لازم دارد → **روی CI بیلد کن** |
| 🔄 **آپدیت آپ‌استریم** | اگه سورس رسمی آپدیت شد و پچ conflict داد → Actions با `::error::Patch failed to apply` فیل می‌شود → پچ را `rebase` کن |
| 🏷️ **فلگ جدید** | حتماً در `BuildVars.java` تعریف شود + در `vipmod.patch` اعمال شود |
| ✅ **تست قبل از پوش** | `git apply --check ../vipmod.patch` در دایرکتوری telegram |

---

## 🔐 امنیت

| لایه | اقدام |
|:---:|---|
| 🔑 **API Credentials** | هرگز در کد هاردکد نیست — فقط **GitHub Encrypted Secrets** (`API_ID` / `API_HASH`) و در لحظه بیلد تزریق می‌شوند |
| 🕵️ **لاگ بیلد** | مقادیر حساس توسط GitHub به‌صورت `***` ماسک می‌شوند |
| 🌐 **بدون سرور واسط** | هیچ ترافیکی از سرور شخص ثالث رد نمی‌شود — مستقیم به MTProto تلگرام |
| 📖 **کد باز** | کل پچ (`vipmod.patch`) قابل بازبینی و آدیت کامل است |
| ✍️ **امضای APK** | با کلید توسعه‌ای رسمی (برای مصرف شخصی)؛ برای انتشار عمومی، keystore اختصاصی بساز |

---

## ⚠️ نکات قانونی و اخلاقی

> **مهم:** این یک پروژه‌ی **شخصی** است؛ مسئولیت استفاده کاملاً با کاربره.

- 🧑‍⚖️ استفاده از این کلاینت روی **حساب شخصی خودتان** مجاز است
- 🤝 مودهای **ضدحذف/گوست** باعث می‌شوند طرف مقابل متوجه نشود — **با احترام و اخلاق** استفاده کنید
- 🔑 کلید API از **حساب شخصی خودِ شماست** → هر سوءاستفاده‌ای (اسپم، تبلیغ) مستقیماً به صاحب حساب برمی‌گردد
- 📜 لایسنس **MIT** — آزاد در استفاده، تغییر، و توزیع (با حفظ کپی‌رایت)
- ⚖️ این پروژه وابسته به [Telegram for Android](https://github.com/DrKLO/Telegram) است که تحت **GPLv2** لایسنس شده — توزیع مجدد APK باید با GPLv2 سازگار باشد

---

## ❓ سؤالات متداول

<details>
<summary><b>❓ آیا حساب من بن می‌شود؟</b></summary>
<br/>
برخلاف ربات‌ها و کلاینت‌های غیررسمی، VipGram دقیقاً با پروتکل و سورس رسمی کار می‌کند؛ مودها فقط رفتار **نمایشی/شبکه‌ای سمت کلاینت** را تغییر می‌دهند. با این حال **هیچ تضمینی وجود ندارد** — ریسک اسپم با کلید API خودت است. استفاده محتاطانه و غیراسپم توصیه می‌شود.
</details>

<details>
<summary><b>❓ آپدیت تلگرام می‌آید، VipGram چه می‌شود؟</b></summary>
<br/>
بیلد همیشه از **آخرین** سورس رسمی (DrKLO/Telegram) کلون می‌شود. اگه ساختار فایل‌ها عوض شده باشد، پچ conflict می‌دهد و بیلد فیل می‌شود — باید پچ با نسخه‌ی جدید هماهنگ (rebase) شود.
</details>

<details>
<summary><b>❓ قابلیت پرمیوم واقعی باز می‌شود؟</b></summary>
<br/>
**خیر.** نشان ⭐ فقط **ظاهری** است. آپلود ۴ گیگی، استیکر پرمیوم، ترجمه و... سمت **سرور** کنترل می‌شوند و با مود کلاینت باز نمی‌شوند.
</details>

<details>
<summary><b>❓ چرا فقط arm64-v8a؟</b></summary>
<br/>
برای **نصف شدن حجم APK** و حذف کدهای نیتیو ۳۲بیتی. گوشی‌های اندروید ۸ به بعد (از ۲۰۱۷) همه `arm64-v8a` هستند. اگر گوشی قدیمی دارید، از تلگرام رسمی استفاده کنید.
</details>

<details>
<summary><b>❓ چطور گزارش باگ بدم یا پیشنهادی بدم؟</b></summary>
<br/>
از تب **Issues** این ریپو استفاده کنید. لطفاً قبل از باز کردن Issue، لیست Issues موجود را چک کنید تا تکرار نشود. قالب Issue توصیه شده:
- **توضیح مختصر باگ/پیشنهاد**
- **مراحل بازتولید** (اگه باگه)
- **اسکرین‌شات/لاگ** (ممکنه)
- **نسخه اندروید / دستگاه**
</details>

<details>
<summary><b>❓ چطور از بیلدهای قدیمی دانلود کنم؟</b></summary>
<br/>
تمام بیلدهای موفق به‌عنوان **GitHub Release** منتشر می‌شوند. به تب [Releases](../../releases) بروید و نسخه دلخواه را دانلود کنید. Artifactها در Actions فقط ۳۰ روز نگهداری می‌شوند.
</details>

<details>
<summary><b>❓ آیا می‌توانم این را برای iOS بیلد کنم؟</b></summary>
<br/>
**خیر.** این پچ فقط برای **Android** (سورس DrKLO/Telegram) نوشته شده. iOS کدبیس کاملاً جداگانه دارد.
</details>

---

## 📂 ساختار ریپو

```text
VipGram/
├── vipmod.patch                     # 🧩 تمام ۸ مود — تک‌پچ قابل آدیت
├── .github/
│   ├── workflows/
│   │   └── build.yml                # 🤖 بیلد خودکار GitHub Actions
│   └── assets/
│       ├── logo.svg                 # 🎨 لوگوی وکتور (مستقل از رزولوشن)
│       └── logo.png                 # 🎨 لوگوی رندرد (1024×1024)
├── README.md                        # 📖 همین فایل
└── LICENSE                          # ⚖️ MIT License
```

---

## 🤝 مشارکت

این پروژه شخصی است و PR عمومی نمی‌پذیرد. اما:
- 🐛 **گزارش باگ** → [Issues](../../issues/new/choose)
- 💡 **پیشنهاد فیچر** → [Issues](../../issues/new/choose) با برچسب `enhancement`
- 🔒 **گزارش امنیتی** → مستقیم به [@iprez](https://t.me/iprez) در تلگرام

---

## 📜 لایسنس

این پروژه تحت لایسنس **MIT** منتشر شده است. دیدن فایل [LICENSE](LICENSE) برای جزئیات.

> ⚠️ نکته: این پروژه بر پایه [Telegram for Android](https://github.com/DrKLO/Telegram) ساخته شده که تحت **GPLv2** لایسنس شده است. توزیع باینری باید با GPLv2 سازگار باشد.

---

<div align="center">

---

### 📊 آمار ریپو

![Stars](https://img.shields.io/github/stars/abapqlcm/VipGram?style=social)
![Forks](https://img.shields.io/github/forks/abapqlcm/VipGram?style=social)
![Watchers](https://img.shields.io/github/watchers/abapqlcm/VipGram?style=social)
![Issues](https://img.shields.io/github/issues/abapqlcm/VipGram?style=flat-square)
![Last Commit](https://img.shields.io/github/last-commit/abapqlcm/VipGram?style=flat-square)

---

**ساخته‌شده با ❤️ و ☕ توسط [@iprez](https://t.me/iprez)**

*اگه این پروژه برات جالب بود، یه ⭐ یادت نره*

</div>

---

<sup>📅 آخرین بروزرسانی: ۲۰۲۶-۱۱-۲۱ · 📝 نسخه README: 2.0.0</sup>