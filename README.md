<div align="center">

# 👑 VipGram

### تلگرام شخصی‌سازی‌شده برای اندروید — بدون تبلیغ، بدون حذف، بدون محدودیت

بر پایه‌ی سورس رسمی [Telegram for Android](https://github.com/DrKLO/Telegram) · نسخه ۱۲.۱۰.۱

<br/>

![Platform](https://img.shields.io/badge/platform-Android_8.0%2B-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Telegram](https://img.shields.io/badge/based%20on-Telegram%2012.10.1-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)
![Arch](https://img.shields.io/badge/arch-arm64--v8a-0A0A0A?style=for-the-badge)
![CI](https://img.shields.io/badge/build-GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Mod](https://img.shields.io/badge/patches-8%20mods-D4AF37?style=for-the-badge)
![Developer](https://img.shields.io/badge/developer-@iprez-FFD700?style=for-the-badge&logo=telegram&logoColor=000)

</div>

---

> ✨ **VipGram** یک کلاینت تلگرام **کاملاً شخصی** است که همه‌ی تغییراتش فقط در **سمت کلاینت** (اپ شما) اعمال می‌شه — بدون هیچ سرور واسطه، بدون تغییر در زیرساخت تلگرام.
> کنار تلگرام اصلی روی **همون گوشی** نصب می‌شه و تداخلی نداره.

---

## 📑 فهرست

- [✨ قابلیت‌ها](#-قابلیت‌ها)
- [🎨 برندینگ](#-برندینگ)
- [🏗️ معماری و بیلد](#-معماری-و-بیلد)
- [📥 نصب](#-نصب)
- [🛠️ توسعه](#-توسعه--تغییر-مودها)
- [🔐 امنیت](#-امنیت)
- [⚠️ نکات قانونی](#-نکات-قانونی--اخلاقی)
- [❓ سؤالات متداول](#-سؤالات-متداول)

---

## ✨ قابلیت‌ها

همه‌ی مودها با فلگ‌های `boolean` در `BuildVars.java` کنترل می‌شن و به‌صورت **تک‌پچ** در `vipmod.patch` نگهداری می‌شن.

| | قابلیت | وضعیت | توضیح |
|:---:|---|:---:|---|
| 🛡️ | **ضدحذف پیام** | `NO_ADS_AND_KEEP_MESSAGES` | پیام‌های حذف‌شده «برای همه» نزد شما باقی می‌مونن — هم چت عادی، هم کانال |
| 🚫 | **حذف تبلیغات** | `NO_ADS_AND_KEEP_MESSAGES` | نه بنر اسپانسر، نه پیام‌های PSA — هیچ‌چی |
| ⚡ | **دانلود سریع‌تر** | `SPEED_BOOST` | ۱۲ درخواست موازی + چانک ۵۱۲KB (به‌جای ۴×۱۲۸KB) |
| 🚀 | **آپلود سریع‌تر** | `SPEED_BOOST` | چانک ۲MB + سقف ۱۶ درخواست موازی — بدون فلاد کردن سرور |
| 📸 | **اسکرین‌شات آزاد** | `NO_SCREENSHOT_RESTRICTION` | برداشتن `FLAG_SECURE` از چت مخفی، ویوور مدیا و مدیای سکرت |
| ♾️ | **عکس تایمردار همیشگی** | `KEEP_SELF_DESTRUCT_MEDIA` | مدیای self-destruct باز می‌مونه؛ ذخیره/فوروارد فعال؛ بدون اطلاع به طرف |
| ⭐ | **نشان پرمیوم** | `LOCAL_PREMIUM_LOOK` | ستاره‌ی پرمیوم برای حساب خودت — کاملاً محلی و ظاهری |
| 👻 | **گوست استوری** | `GHOST_STORY` | استوری ببین، ولی طرف نفهمه دیدی — بدون ارسال `incrementStoryViews` |

<details>
<summary><b>🔍 جزئیات فنی هر مود</b> (باز کن)</summary>

<br/>

- **🛡️ ضدحذف:** اپ دستور `updateDeleteMessages` و `updateDeleteChannelMessages` رو از سرور دریافت می‌کنه ولی نادیده می‌گیره (`continue`) — پیام فقط در دیتابیس **شما** می‌مونه.
- **🚫 ضدتبلیغ:** دو نقطه قطع شده — ۱) `promoDialogId` هرگز ست نمی‌شه ۲) `getSponsoredMessages()` مقدار `null` برمی‌گردونه.
- **⚡🚀 بوست سرعت:** در `FileLoadOperation` و `FileUploadOperation`. باگ نسخه‌های قبل فیکس شد: چانک ۶۴KB باعث ۲۵۶ درخواست موازی و flow-control سمت سرور می‌شد → حالا `Math.min(16, ...)` با چانک ۲MB.
- **📸 اسکرین‌شات:** `FLAG_SECURE` در `ChatActivity`، `PhotoViewer` و `SecretMediaViewer` غیرفعال.
- **♾️ مدیای تایمردار:** `sendSecretMessageRead` مقدار `null` می‌ده → تایمر استارت نمی‌خوره و TTL به سرور گزارش نمی‌شه؛ `galleryButton` مخفی نمی‌شه.
- **⭐ پرمیوم لوکال:** در `putUser` فقط برای اکانت خودت `user.premium = true` + `UserConfig.isPremium()` → `true`. **هیچ** قابلیت واقعی پرمیوم (آپلود بزرگ‌تر، استیکر و...) باز نمی‌شه.
- **👻 گوست استوری:** علامت‌گذاری seen فقط محلی؛ درخواست `TL_stories_incrementStoryViews` ارسال نمی‌شه.

</details>

---

## 🎨 برندینگ

| مورد | مقدار |
|---|---|
| 📛 نام اپ | **VipGram** |
| 📦 Package | `com.vipgram.rez` |
| 🏗️ معماری | فقط `arm64-v8a` (APK سبک‌تر) |
| 👨‍💻 توسعه‌دهنده | [@iprez](https://t.me/iprez) |

> 💡 چون package name متفاوته، VipGram **کنار** تلگرام رسمی قابل نصبه و دو حساب همزمان می‌تونن فعال باشن.

---

## 🏗️ معماری و بیلد

بیلد **کاملاً خودکار** روی GitHub Actions انجام می‌شه — هیچ نیازی به کامپیوتر قوی نداری.

```
   ┌──────────────────────────────────────────────────┐
   │            🤖  GitHub Actions Runner             │
   │            ubuntu-latest · JDK 21 · NDK          │
   ├──────────────────────────────────────────────────┤
   │                                                  │
   │   1️⃣  کلون سورس رسمی Telegram (DrKLO/Telegram)   │
   │              + همه‌ی submodules                   │
   │                      │                           │
   │   2️⃣  اعمال vipmod.patch (۸ مود)                 │
   │                      │                           │
   │   3️⃣  تزریق API_ID / API_HASH از Secrets         │
   │                      │                           │
   │   4️⃣  کامپایل نیتیو C++ + Gradle                 │
   │        assembleAfatRelease                       │
   │                      │                           │
   │   5️⃣  امضا + آپلود Artifact                      │
   │                                                  │
   └──────────────────────┬───────────────────────────┘
                          ▼
              📦  VipGram-APK (امضاشده، آماده نصب)
```

**⏱️ زمان بیلد:** حدود ۵۰ دقیقه (کامپایل نیتیو سنگین‌ترین مرحله‌ست؛ روی رانر ۲ هسته‌ای با `workers.max=2 + Xmx4g` بهینه شده).

**🔀 تریگرها:**
- `push` به `main` — فقط وقتی `vipmod.patch` یا `build.yml` تغییر کرده باشن
- `workflow_dispatch` — اجرای دستی از تب Actions

---

## 📥 نصب

۱. به تب **[Actions](../../actions)** ریپو برو → آخرین بیلد سبز ✅
۲. از بخش **Artifacts** فایل `VipGram-APK` رو دانلود کن
۳. zip رو استخراج کن → APK رو نصب کن (اجازه‌ی *منابع ناشناس* لازم می‌شه)
۴. با شماره‌ی خودت لاگین کن — تمام! 😎

> 🍃 اولین اجرا ممکنه چند ثانیه طول بکشه (ساخت دیتابیس محلی).

---

## 🛠️ توسعه / تغییر مودها

```bash
# 1. کلون کن
git clone https://github.com/abapqlcm/VipGram.git

# 2. سورس تلگرام رو کلون کن و پچ رو اعمال کن
git clone https://github.com/DrKLO/Telegram.git telegram
cd telegram && git submodule update --init --depth 1
git apply ../vipmod.patch

# 3. تغییر بده، دیف بگیر و پچ رو آپدیت کن
git diff > ../vipmod.patch

# 4. پوش کن → بیلد خودکار شروع می‌شه
git push origin main
```

**نکات مهم:**
- سورس تلگرام ~۳GB و بیلد به ۴-۸GB RAM نیاز داره → بیلد رو روی گیت‌هاب انجام بده
- اگه سورس رسمی آپدیت بشه و پچ نخورد، Actions با `::error::Patch failed to apply` فیل می‌شه → باید پچ رو rebase کنی
- فلگ جدید حتماً در `BuildVars.java` تعریف بشه

---

## 🔐 امنیت

| | |
|---|---|
| 🔑 **API Credentials** | هرگز در کد هاردکد نیست — فقط به‌صورت **GitHub Encrypted Secrets** (`API_ID` / `API_HASH`) و در لحظه‌ی بیلد با `sed` تزریق می‌شه |
| 🕵️ **لاگ بیلد** | مقادیر حساس توسط GitHub به‌صورت `***` ماسک می‌شن |
| 🌐 **بدون سرور واسط** | هیچ ترافیکی از سرور شخص ثالث رد نمی‌شه — مستقیم به MTProto تلگرام |
| 📖 **کد باز** | کل پچ (`vipmod.patch`) قابل بازبینی و آدیت است |

---

## ⚠️ نکات قانونی / اخلاقی

- 🧑‍⚖️ این یک پروژه‌ی **شخصی** است؛ مسئولیت استفاده کاملاً با کاربره
- 🤝 مودهای ضدحذف/گوست باعث می‌شن طرف مقابل متوجه نشه — **با احترام و اخلاق** استفاده کن
- 🔑 کلید API از حساب شخصیه → هر سوءاستفاده‌ای (اسپم) به خود صاحب حساب برمی‌گرده
- ✍️ امضای APK با کلید توسعه‌ای رسمی هست — برای مصرف شخصی اوکیه؛ برای انتشار عمومی، keystore اختصاصی بساز

---

## ❓ سؤالات متداول

<details>
<summary><b>آیا حسابم بن می‌شه؟</b></summary>
برخلاف ربات‌ها و کلاینت‌های متفرقه، VipGram دقیقاً با پروتکل و سورس رسمی کار می‌کنه؛ مودها فقط رفتار نمایشی/شبکه‌ای سمت کلاینت رو عوض می‌کنن. با این حال هیچ تضمینی وجود نداره — ریسک اسپم با کلید خودته.
</details>

<details>
<summary><b>آپدیت تلگرام میاد، VipGram چی؟</b></summary>
بیلد همیشه از **آخرین** سورس رسمی کلون می‌شه. اگه ساختار فایل‌ها عوض شده باشه، پچ conflict می‌ده و بیلد فیل می‌شه — باید پچ با نسخه‌ی جدید هماهنگ بشه.
</details>

<details>
<summary><b>قابلیت پرمیوم واقعی باز می‌شه؟</b></summary>
خیر. نشان ⭐ فقط ظاهریه. آپلود ۴ گیگی، استیکر پرمیوم و... سمت **سرور** کنترل می‌شن و با مود کلاینت باز نمی‌شن.
</details>

<details>
<summary><b>چرا فقط arm64؟</b></summary>
برای نصف شدن حجم APK. گوشی‌های اندروید ۸ به بعد همه arm64 هستن.
</details>

---

## 📂 ساختار ریپو

```
VipGram/
├── vipmod.patch                  # 🧩 تمام ۸ مود — تک‌پچ قابل آدیت
├── .github/workflows/
│   └── build.yml                 # 🤖 بیلد خودکار GitHub Actions
└── README.md                     # 📖 همین فایل
```

---

<div align="center">

**ساخته‌شده با ❤️ و ☕ توسط [@iprez](https://t.me/iprez)**

*اگه این پروژه برات جالب بود، یه ⭐ یادت نره*

</div>
