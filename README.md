# VipGram 🚀

تلگرام مود شده شخصی — بر پایه سورس رسمی Telegram for Android

## مودها (توی `vipmod.patch`)

| مود | توضیح | فایل |
|---|---|---|
| 🛡️ ضدحذف | پیام‌هایی که طرف مقابل «برای همه» پاک می‌کنه، نزد تو می‌مونن | `MessagesController.java` |
| 🚫 ضدتبلیغ | اسپانسری‌های کانال‌ها هرگز لود نمی‌شن | `MessagesController.java` |
| ⚡ سرعت دانلود | ۸ درخواست موازی + چانک ۵۱۲KB (به جای ۴ و ۱۲۸KB) | `FileLoadOperation.java` |
| ⚡ سرعت آپلود | چانک آپلود ×۴ بزرگتر | `FileUploadOperation.java` |
| 🎨 برند | اسم: VipGram — پکیج: `com.vipgram.rez` | `strings.xml` + `gradle.properties` |

کلیدهای کنترل توی `BuildVars.java`:
```java
public static boolean NO_ADS_AND_KEEP_MESSAGES = true;  // ضدحذف + ضدتبلیغ
public static boolean SPEED_BOOST = true;               // بوست سرعت
```

## بیلد خودکار با GitHub Actions

هیچ کاری لازم نیست! با هر تغییر توی `vipmod.patch` یا ورک‌فلو، گیت‌هاب:
1. آخرین سورس رسمی تلگرام رو کلون می‌کنه
2. پچ VipMod رو اعمال می‌کنه
3. APK امضاشده release می‌سازه
4. توی تب **Actions → آخرین run → Artifacts** دانلود کن (`VipGram-APK`)

بیلد دستی: تب **Actions** → **Build VipGram APK** → **Run workflow**

## نصب روی گوشی

1. APK رو از آرتیفکت‌ها دانلود کن
2. اگه تلگرام اصلی داری، VipGram جدا نصب می‌شه (پکیج متفاوته) — هیچ تداخلی نیست
3. اولین اجرا ممکنه چند ثانیه طول بکشه

## نکته‌ها

- **api_id/api_hash**: فعلاً از مقادیر عمومی سورس استفاده شده. برای انتشار عمومی حتماً از [my.telegram.org](https://my.telegram.org) مال خودت رو بگیر و توی `BuildVars.java` جایگزین کن
- **امضا**: کلید release داخل ریپو DrKLO فقط برای توسعه است؛ برای مصرف روزمره مشکلی نداره
- **آپدیت تلگرام**: اگه سورس رسمی تغییر کنه و پچ نخوره، Actions ارور می‌ده — یعنی باید پچ رو آپدیت کنیم
