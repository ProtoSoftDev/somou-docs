# مستودع وثائق منظومة سمو الذهبية للنقل البري
# Somou Al Thahabiya Land Transport - Documentation Repository

---

## 📌 نبذة عامة | Overview

مرحباً بك في المستودع الرسمي للتوثيق الفني والتشغيلي لمنظومة **سمو الذهبية للنقل البري (Somou Al Thahabiya Land Transport)**.
يحتوي هذا المستودع على جميع الأدلة المعمارية، مراجع واجهات برمجة التطبيقات (APIs)، توثيق WhatsApp Cloud API، إرشادات تكامل الرسائل القصيرة (SMS Msegat)، لقطات الشاشة لواجهات المنظومة، وأدلة تشغيل النظام.

---

## 🌐 النطاقات الحالية (Active Domains)

| الخدمة / الخدمة الرقمية | الرابط المؤقت المعتمد | الوصف |
| :--- | :--- | :--- |
| **واجهة برمجة التطبيقات (API)** | [https://api.aldikka.com](https://api.aldikka.com) | خادم الواجهات الخلفية (Node.js/Express) ومعالجة الطلبات والحجوزات |
| **لوحة تحكم الإدارة (Admin)** | [https://admin.aldikka.com](https://admin.aldikka.com) | لوحة القيادة المركزية والعمليات والتقارير والشركات والسائقين |
| **بوابة الحجز والموقع التعريفي (App)** | [https://app.aldikka.com](https://app.aldikka.com) | بوابة الحجز المباشر (Guest Booking) وعرض الأسطول والخدمات |

---

## 📂 هيكل مستودع الوثائق | Repository Structure

```plaintext
somou-docs/
├── architecture/                     # المعمارية الفنية للنظام وتدفق العمليات
│   ├── somou_al_thahabiya_system_architecture.md   # الوثيقة المعمارية الشاملة للمنظومة
│   ├── PARTNER_NOTIFICATIONS_ARCHITECTURE.md       # معمارية وتدفق إشعارات السائقين والشركاء
│   └── message_delivery_flow.md                    # تدفق إرسال الرسائل والتنبيهات
│
├── api-reference/                    # توثيق واجهات برمجة التطبيقات (REST APIs)
│   └── API_ENDPOINTS_DOCUMENTATION.md             # مرجع تفصيلي لكافة المسارات، النماذج، والتراخيص
│
├── guides/                           # أدلة التكامل والتجارب الميدانية
│   ├── msegat-api-curl-testing.md                 # دليل اختبار بوابات الرسائل القصيرة (Msegat SMS)
│   ├── WHATSAPP_INTERACTIVE_CURL_TESTS.md         # اختبارات رسائل واتساب التفاعلية عبر cURL
│   ├── HOW_TO_ADD_VEHICLE_IMAGES.md               # دليل إدارة ورفع وسائط صور المركبات
│   └── guest-booking-flow.md                      # مسار وتجربة حجز الضيوف بدون تسجيل مسبق
│
├── screenshots/                      # صور ولقطات شاشات واجهات المستخدم (75 لقطة)
│   ├── 01_homepage_hero.png .. 21_guest_booking.png # لقطات الموقع العام ولوحة الإدارة
│   ├── customer_*.png                             # لقطات بوابة العميل
│   ├── partner_*.png                              # لقطات بوابة الشريك / السائق
│   └── company_*.png                              # لقطات بوابة الشركات والمؤسسات B2B
│
├── user-manual/                      # أدلة التشغيل والتثبيت للمستخدمين
│   ├── system_setup_user_manual.html              # دليل الإعداد كصفحة ويب تفاعلية
│   └── system_setup_user_manual.pdf               # دليل الإعداد بصيغة مستند PDF
│
└── whatsapp_docs/                    # المرجع الكامل لتكامل WhatsApp Cloud API
    ├── overview.md                   # نظرة عامة وبداية سريعة
    ├── webhooks/                     # إعداد مستقبلات أحداث واتساب (Webhooks)
    ├── messages/                     # إرسال الرسائل (نص، صور، تفاعلية، أزرار)
    ├── templates/                    # إدارة واعتماد قوالب الرسائل وتصنيفاتها
    ├── solution-providers/           # تكامل مقدمي الحلول التكنولوجية
    └── business-phone-numbers/       # إدارة أرقام الأعمال والتحقق بخطوتين
```

---

## 🏗️ مكونات منظومة سمو الذهبية الرئيسية

1. **الخادم الخلفي (Core Backend Engine):**
   - مبني بتقنية TypeScript و Express.
   - يدعم قواعد بيانات PostgreSQL / Supabase و Redis للتخزين المؤقت وطوابير المهام.
   - تكامل متقدم مع Google Maps Platform لحساب المسارات والتسعير والتتبع الحي.
   - بوابات دفع إلكتروني، وإشعارات Firebase Cloud Messaging (FCM)، وتنبيهات WhatsApp & SMS.

2. **لوحة تحكم الإدارة (Admin Dashboard):**
   - منصة Next.js مركزية لإدارة الأسطول، السائقين، الشركات، التسعير، المحفظة، والفواتير الضريبية.

3. **بوابة الويب وحجز الضيوف (Web Marketing & Booking Portal):**
   - واجهة Next.js سريعة الاستجابة توفر مسار حجز سلس للعملاء والضيوف بدون الحاجة لتثبيت التطبيق.

4. **تطبيقات الهواتف الذكية (Mobile Ecosystem - React Native & Expo):**
   - **تطبيق العميل (Customer App):** رحلات VIP، تتبع حي، خيارات دفع متعددة، وإدارة العناوين.
   - **تطبيق السائق (Driver App):** استلام وتحديث الرحلات، الملاحة الذكية، ومحفظة الأرباح.
   - **تطبيق الشركات (Corporate B2B App):** إدارة حسابات الموظفين، الحد الائتماني، وتصدير الفواتير.

---

## 🔒 الأمان وحماية البيانات

- استخدام توثيق JWT مشفر مع صلاحيات دقيقة مبنية على الأدوار (RBAC).
- دعم التحقق بخطوتين (2FA) ورموز الدخول لمرة واحدة (OTP) عبر الرسائل النصية وواتساب.
- تشفير كامل لجميع الاتصالات عبر HTTPS/TLS.

---

*جميع الحقوق محفوظة © 2026 | سمو الذهبية للنقل البري (Somou Al Thahabiya Land Transport)*
