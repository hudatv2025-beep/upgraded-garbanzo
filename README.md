# 🔴 Tango Live Clone — تطبيق بث مباشر كامل

تطبيق بث مباشر (Live Streaming) مشابه لتطبيق Tango Live، مبني بـ **React Native** ويعتمد 100% على **خدمات مجانية**.

---

## 🎯 الميزات الموجودة

| الميزة | الوصف |
|--------|-------|
| 🔴 **بث مباشر** | بث فيديو مباشر بجودة عالية عبر Agora.io |
| 💬 **دردشة نصية** | دردشة فورية في الوقت الحقيقي عبر Firebase |
| 🎁 **هدايا افتراضية** | إرسال هدايا (وردة، قلب، تاج، ألماس، صاروخ) مع أنيميشن |
| 🔐 **تسجيل دخول** | Google Sign-In + دخول كضيف |
| 👤 **ملف شخصي** | صورة، اسم، عدد المتابعين، الرصيد، سجل البثوث |
| 🔔 **اكتشاف البثوث** | قائمة البثوث المباشرة مع عدد المشاهدين |
| 📱 **تصميم احترافي** | UI/UX مطابق لتطبيقات البث العالمية |

---

## 🆓 الخدمات المجانية المستخدمة

| الخدمة | الاستخدام | الحد المجاني |
|--------|----------|-------------|
| **Firebase** | Auth + Firestore + Storage + FCM | Spark Plan — مجاني |
| **Agora.io** | بث الفيديو المباشر | 10,000 دقيقة/شهر |

---

## ⚙️ خطوات الإعداد

### 1️⃣ تثبيت المتطلبات

```bash
# Node.js 18+
npm install -g react-native-cli

# Android Studio (للأندرويد)
# Xcode (للي iOS — فقط على Mac)
```

### 2️⃣ إنشاء مشروع Firebase

1. اذهب إلى [Firebase Console](https://console.firebase.google.com/)
2. أنشئ مشروع جديد
3. أضف تطبيق Android/iOS
4. حمّل ملف `google-services.json` (Android) أو `GoogleService-Info.plist` (iOS)
5. فعّل:
   - **Authentication** → Google Sign-In
   - **Firestore Database** → ابدأ في وضع الاختبار
   - **Storage**
   - **Cloud Messaging**

### 3️⃣ إنشاء حساب Agora

1. اذهب إلى [Agora Console](https://console.agora.io/)
2. أنشئ مشروع جديد
3. احصل على **App ID** مجاني
4. (اختياري) فعّل Token Authentication للأمان

### 4️⃣ تعديل الإعدادات

افتح `src/services/firebase.js` واستبدل القيم:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID",
};
```

افتح `src/services/agora.js` واستبدل:
```javascript
const AGORA_APP_ID = "YOUR_AGORA_APP_ID";
```

افتح `src/context/AuthContext.js` واستبدل:
```javascript
webClientId: 'YOUR_WEB_CLIENT_ID.apps.googleusercontent.com',
```

### 5️⃣ تثبيت الحزم

```bash
cd tango-live-app
npm install
# أو
yarn install
```

### 6️⃣ تشغيل على Android

```bash
npx react-native run-android
```

### 7️⃣ تشغيل على iOS (Mac فقط)

```bash
cd ios && pod install && cd ..
npx react-native run-ios
```

---

## 📁 هيكل المشروع

```
tango-live-app/
├── src/
│   ├── components/       # مكونات قابلة لإعادة الاستخدام
│   ├── screens/          # الشاشات
│   │   ├── LoginScreen.js
│   │   ├── HomeScreen.js      (اكتشاف البثوث)
│   │   ├── GoLiveScreen.js    (بدء البث)
│   │   ├── LiveScreen.js      (مشاهدة البث + دردشة + هدايا)
│   │   └── ProfileScreen.js   (الملف الشخصي)
│   ├── navigation/       # التنقل بين الشاشات
│   ├── services/         # Firebase + Agora
│   └── context/          # إدارة حالة المستخدم
├── App.js
├── package.json
└── README.md
```

---

## 🔥 قواعد Firestore (Security Rules)

افتح Firestore Database → Rules والصق:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

> ⚠️ للإنتاج، شدد القواعد أكثر!

---

## 🚀 خطوات النشر على المتاجر

### Google Play Store:
1. `cd android && ./gradlew assembleRelease`
2. احصل على ملف `app-release.apk` أو `app.aab`
3. ارفعه على [Google Play Console](https://play.google.com/console)

### Apple App Store:
1. افتح `ios/TangoLive.xcworkspace` في Xcode
2. Product → Archive
3. ارفع عبر Organizer إلى App Store Connect

---

## 💡 نصائح للتطوير

- **Token Server**: للإنتاج، أنشئ سيرفر Node.js بسيط يولد Agora tokens (يمكن استخدام Firebase Functions مجاناً)
- **Moderation**: أضف AI moderation للدردشة باستخدام Perspective API (مجاني)
- **Payments**: لشراء Coins، ربط Stripe أو PayPal
- **CDN**: للصور الكثيرة، استخدم Cloudinary Free Tier
- **Analytics**: Firebase Analytics مجاني ومدمج

---

## 📞 دعم

إذا واجهت أي مشكلة:
1. تأكد من إعدادات Firebase صحيحة
2. تأكد من App ID الخاص بـ Agora
3. تأكد من صلاحيات الكاميرا والمايكروفون
4. راجع `adb logcat` (Android) أو Xcode logs (iOS)

---

## 📄 الترخيص
.

MIT License — استخدمه تجارياً أو شخصياً كما تشاء!

**تم البناء بـ ❤️ باستخدام خدمات مجانية 100%**
