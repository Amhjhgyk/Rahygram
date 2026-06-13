# گرفتن APK بدون Android Studio

راحت‌ترین روش برای تو این است که پروژه را داخل GitHub بگذاری و از GitHub Actions یا Codemagic خروجی APK بگیری.

## روش پیشنهادی: GitHub Actions

1. در GitHub یک repository جدید بساز.
2. کل پوشه پروژه را داخل آن آپلود کن.
3. وارد صفحه repository شو.
4. از بالا روی **Actions** بزن.
5. workflow به نام **Android Debug APK** را انتخاب کن.
6. روی **Run workflow** بزن.
7. صبر کن build کامل شود.
8. وارد همان build شو.
9. پایین صفحه، از بخش **Artifacts** فایل `rahygram-debug-apk` را دانلود کن.
10. فایل zip را باز کن؛ داخلش APK برنامه است.

## روش دوم: Codemagic

1. پروژه را در GitHub آپلود کن.
2. وارد سایت Codemagic شو.
3. GitHub را وصل کن.
4. repository پروژه را انتخاب کن.
5. Codemagic فایل `codemagic.yaml` را می‌شناسد.
6. workflow به نام **RahyGram Debug APK** را اجرا کن.
7. بعد از اتمام build، APK را از بخش **Artifacts** دانلود کن.

## اگر Firebase واقعی می‌خواهی

برای نسخه دمو لازم نیست کاری کنی.

برای نسخه Firebase:

1. از Firebase فایل `google-services.json` را بگیر.
2. آن را Base64 کن.
3. در GitHub Secrets یا Codemagic Environment Variables این نام را بساز:

```text
GOOGLE_SERVICES_JSON_BASE64
```

4. در فایل `app/build.gradle.kts` مقدار زیر را از `false` به `true` تغییر بده:

```kotlin
buildConfigField("Boolean", "USE_FIREBASE_BACKEND", "true")
```

5. دوباره build بگیر.

## نکته

این خروجی **Debug APK** است و برای تست روی گوشی مناسب است. برای انتشار رسمی در بازار، مایکت یا Google Play باید نسخه Release امضاشده ساخته شود.
