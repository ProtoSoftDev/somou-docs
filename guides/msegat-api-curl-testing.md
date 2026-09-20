# 📨 MSEGAT API — دليل الاختبار بـ curl

> وثيقة شاملة لاختبار جميع نقاط نهاية MSEGAT المستخدمة في المشروع عبر أوامر `curl` جاهزة للتشغيل.

---

## 🔐 بيانات الاعتماد (Credentials)

```
USERNAME   = GoldenSumou
API_KEY    = 733B457A7418F8C332E5F835EA57D0E0
SENDER     = GoSumou
SENDER_AD  = GoSumou-AD
LANG       = Ar
TEST_PHONE = 966591443542   ← رقم المستلم التجريبي (بدون +)
```

---

## 📋 فهرس نقاط النهاية

| #  | النهاية             | الملف (MSEGAT)             | الوصف                                    |
|----|---------------------|----------------------------|------------------------------------------|
| 1  | `sendOTPCode.php`   | `msegat.service.ts`        | إرسال OTP عبر Msegat (يُولّد الكود تلقائيًا) |
| 2  | `verifyOTPCode.php` | `msegat.service.ts`        | التحقق من OTP بالـ session ID              |
| 3  | `sendsms.php`       | `msegat-sms.service.ts`    | إرسال رسالة SMS عادية / مخصصة             |
| 4  | `sendsms.php`       | `msegat-sms.service.ts`    | إرسال OTP مجاني (sender: auth-mseg)       |
| 5  | `sendVars.php`      | `msegat-sms.service.ts`    | إرسال رسائل متغيرة لأكثر من مستلم          |
| 6  | `Credits.php`       | `msegat-sms.service.ts`    | الاستعلام عن الرصيد المتبقي                |

---

## 1️⃣ إرسال OTP — `sendOTPCode.php`

**الوصف:** يُرسل MSEGAT رمز OTP بشكل تلقائي ويُعيد `id` (session ID) لاستخدامه لاحقًا في التحقق.

**الـ Endpoint:**
```
POST https://www.msegat.com/gw/sendOTPCode.php
```

**Payload المُستخدم في الكود:**
```json
{
  "lang": "Ar",
  "userName": "GoldenSumou",
  "number": "966591443542",
  "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
  "userSender": "GoSumou"
}
```

### ▶️ أمر curl

```bash
curl -s -X POST "https://www.msegat.com/gw/sendOTPCode.php" \
  -H "Content-Type: application/json" \
  -H "lang: Ar" \
  -d '{
    "lang": "Ar",
    "userName": "GoldenSumou",
    "number": "966591443542",
    "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
    "userSender": "GoSumou"
  }'
```

### ✅ استجابة ناجحة متوقعة

```json
{
  "id": 123456789,
  "success": true,
  "code": 1,
  "message": "Success"
}
```

> **⚠️ مهم:** احتفظ بقيمة `id` — ستحتاجه في خطوة التحقق (Endpoint #2).

### ❌ استجابات الخطأ الشائعة

| كود | الرسالة | السبب |
|-----|---------|-------|
| `M001` | Authentication failed | اسم المستخدم أو API Key خاطئ |
| `M002` | Invalid sender | اسم المرسل غير مسجل |
| `M003` | Insufficient balance | الرصيد غير كافٍ |

---

## 2️⃣ التحقق من OTP — `verifyOTPCode.php`

**الوصف:** يُرسل الكود الذي أدخله المستخدم مع الـ `id` المُستلم من الخطوة السابقة للتحقق.

**الـ Endpoint:**
```
POST https://www.msegat.com/gw/verifyOTPCode.php
```

**Payload المُستخدم في الكود:**
```json
{
  "lang": "Ar",
  "userName": "GoldenSumou",
  "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
  "code": "XXXX",
  "id": 123456789,
  "userSender": "GoSumou"
}
```

### ▶️ أمر curl (استبدل `SESSION_ID` و `OTP_CODE` بالقيم الفعلية)

```bash
curl -s -X POST "https://www.msegat.com/gw/verifyOTPCode.php" \
  -H "Content-Type: application/json" \
  -H "lang: Ar" \
  -d '{
    "lang": "Ar",
    "userName": "GoldenSumou",
    "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
    "code": "1234",
    "id": SESSION_ID,
    "userSender": "GoSumou"
  }'
```

> **💡 طريقة بسيطة للاختبار المتسلسل:** شغّل أولًا الأمر #1، استرجع الـ `id`، ثم أدخله هنا مع الكود المُرسل لهاتفك.

### ✅ استجابة ناجحة

```json
{
  "code": 1,
  "message": "Verified"
}
```

### ❌ استجابة كود خاطئ

```json
{
  "code": "M013",
  "message": "Invalid OTP"
}
```

---

## 3️⃣ إرسال SMS عادية — `sendsms.php` (مرسل مخصص)

**الوصف:** إرسال رسالة نصية مخصصة برسالة يحددها المطور. يُستخدم عند `MSEGAT_CUSTOM` أو `MSEGAT_FREE`.

**الـ Endpoint:**
```
POST https://www.msegat.com/gw/sendsms.php
```

**Payload (مع طلب Bulk ID للتتبع):**
```json
{
  "userName": "GoldenSumou",
  "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
  "numbers": "966591443542",
  "userSender": "GoSumou",
  "msg": "رمز التحقق الخاص بك هو: 4829",
  "msgEncoding": "UTF8",
  "reqBulkId": "true"
}
```

### ▶️ أمر curl

```bash
curl -s -X POST "https://www.msegat.com/gw/sendsms.php" \
  -H "Content-Type: application/json" \
  -d '{
    "userName": "GoldenSumou",
    "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
    "numbers": "966591443542",
    "userSender": "GoSumou",
    "msg": "رمز التحقق الخاص بك هو: 4829",
    "msgEncoding": "UTF8",
    "reqBulkId": "true"
  }'
```

### ✅ استجابة ناجحة

```json
{
  "code": 1,
  "message": "Success",
  "id": "987654321"
}
```

---

## 4️⃣ إرسال OTP مجاني — `sendsms.php` (sender: `auth-mseg`)

**الوصف:** طريقة **مجانية** لا تُكلف رصيدًا. تستخدم المرسل الثابت `auth-mseg` وإن كانت تتطلب صياغة بعينها.
هذه هي طريقة **MSEGAT_FREE** في الكود (الـ fallback الأخير).

**قواعد:** 
- الرسالة يجب أن تكون بصيغة: `رمز التحقق: XXXX` (4 أرقام)
- المرسل يجب أن يكون `auth-mseg` (ثابت)

### ▶️ أمر curl

```bash
curl -s -X POST "https://www.msegat.com/gw/sendsms.php" \
  -H "Content-Type: application/json" \
  -d '{
    "userName": "GoldenSumou",
    "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
    "numbers": "966591443542",
    "userSender": "auth-mseg",
    "msg": "رمز التحقق: 4829",
    "msgEncoding": "UTF8"
  }'
```

### ✅ استجابة ناجحة

```json
{
  "code": 1,
  "message": "Success"
}
```

---

## 5️⃣ إرسال رسائل متغيرة — `sendVars.php`

**الوصف:** إرسال رسائل مخصصة لأكثر من مستلم في طلب واحد، مع متغيرات مختلفة لكل مستلم.
يُستخدم في حملات SMS الجماعية المخصصة (`sendPersonalizedSms`).

**الـ Endpoint:**
```
POST https://www.msegat.com/gw/sendVars.php
```

**Payload (مثال: مستلمَين):**
```json
{
  "userName": "GoldenSumou",
  "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
  "numbers": ["966591443542", "966501234567"],
  "userSender": "GoSumou",
  "msg": "مرحبا {name}، رحلتك رقم {trip_id} تبدأ في {time}",
  "msgEncoding": "UTF8",
  "reqBulkId": "true",
  "vars": [
    { "name": "أحمد",  "trip_id": "T001", "time": "08:00" },
    { "name": "محمد", "trip_id": "T002", "time": "09:30" }
  ]
}
```

### ▶️ أمر curl (مستلم واحد تجريبي)

```bash
curl -s -X POST "https://www.msegat.com/gw/sendVars.php" \
  -H "Content-Type: application/json" \
  -d '{
    "userName": "GoldenSumou",
    "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
    "numbers": ["966591443542"],
    "userSender": "GoSumou",
    "msg": "مرحبا {name}، رحلتك رقم {trip_id} جاهزة للانطلاق.",
    "msgEncoding": "UTF8",
    "reqBulkId": "true",
    "vars": [
      { "name": "فلان", "trip_id": "T999" }
    ]
  }'
```

### ✅ استجابة ناجحة

```json
{
  "code": 1,
  "message": "Success",
  "id": "111222333"
}
```

---

## 6️⃣ الاستعلام عن الرصيد — `Credits.php`

**الوصف:** يعرض عدد رسائل SMS المتبقية في الحساب.

**الـ Endpoint:**
```
POST https://www.msegat.com/gw/Credits.php
```

### ▶️ أمر curl

```bash
curl -s -X POST "https://www.msegat.com/gw/Credits.php" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "userName=GoldenSumou" \
  -d "apiKey=733B457A7418F8C332E5F835EA57D0E0" \
  -d "msgEncoding=UTF8"
```

### ✅ استجابة ناجحة

سيكون الرد عبارة عن الرقم الدال على الرصيد فقط (نص يعبر عن الرصيد، وليس JSON)، مثال:

```text
5010
```

> **ملاحظة:** إذا ظهرت مشكلة في المصادقة سيعود كود خطأ نصي مثل `M0001`.

---

## 🔄 سيناريو الاختبار الكامل (خطوة بخطوة)

### الخطوة 1: إرسال OTP

```bash
# شغّل هذا الأمر واحتفظ بـ "id" من الاستجابة
curl -s -X POST "https://www.msegat.com/gw/sendOTPCode.php" \
  -H "Content-Type: application/json" \
  -H "lang: Ar" \
  -d '{
    "lang": "Ar",
    "userName": "GoldenSumou",
    "number": "966591443542",
    "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
    "userSender": "GoSumou"
  }'
```

**📌 الناتج المتوقع:**
```json
{"id": 987654321, "success": true, "code": 1, "message": "Success"}
```

---

### الخطوة 2: التحقق من OTP

```bash
# استبدل 987654321 بالـ id من الخطوة السابقة
# واستبدل 1234 بالكود الذي وصل لهاتفك
curl -s -X POST "https://www.msegat.com/gw/verifyOTPCode.php" \
  -H "Content-Type: application/json" \
  -H "lang: Ar" \
  -d '{
    "lang": "Ar",
    "userName": "GoldenSumou",
    "apiKey": "733B457A7418F8C332E5F835EA57D0E0",
    "code": "1234",
    "id": 987654321,
    "userSender": "GoSumou"
  }'
```

**📌 الناتج المتوقع (نجاح):**
```json
{"code": 1, "message": "Verified"}
```

---

## 🏗️ منطق الـ Fallback في المشروع

عندما تكون `MSEGAT_ENABLED=true`، يتبع الكود هذا الترتيب تلقائيًا:

```
MSEGAT_OTP  ──(فشل)──▶  MSEGAT_CUSTOM  ──(فشل)──▶  MSEGAT_FREE
                          (sendsms.php)               (auth-mseg sender)
                          1 نقطة                      مجاني
```

| الطريقة | الـ Sender | التكلفة | الكود يُولَّد بواسطة |
|---------|-----------|---------|----------------------|
| `MSEGAT_OTP` | `GoSumou` | مدفوع | MSEGAT تلقائيًا |
| `MSEGAT_CUSTOM` | `GoSumou` | 1 نقطة | الخادم (env.otp.length أرقام) |
| `MSEGAT_FREE` | `auth-mseg` | **مجاني** | الخادم (4 أرقام فقط) |

---

## ⚙️ متغيرات البيئة المرتبطة

```env
MSEGAT_ENABLED=false              # تفعيل/تعطيل MSEGAT (true لتشغيله)
MSEGAT_SEND_URL=https://www.msegat.com/gw/sendOTPCode.php
MSEGAT_VERIFY_URL=https://www.msegat.com/gw/verifyOTPCode.php
MSEGAT_USERNAME=GoldenSumou
MSEGAT_API_KEY=733B457A7418F8C332E5F835EA57D0E0
MSEGAT_SENDER=GoSumou
MSEGAT_SENDER-AD=GoSumou-AD
MSEGAT_LANG=Ar
```

> **⚠️ ملاحظة:** في بيئة التطوير الحالية `MSEGAT_ENABLED=false`، مما يعني أن OTP يُرسل عبر WhatsApp/Email.
> لاختبار MSEGAT فعلًا، غيّر `MSEGAT_ENABLED=true` مؤقتًا.

---

## 🪲 نصائح استكشاف الأخطاء

| المشكلة | الحل |
|---------|------|
| الاستجابة فارغة | تحقق من اتصال الإنترنت وصحة الـ URL |
| `Authentication failed` | تحقق من `userName` و `apiKey` |
| `Invalid sender` | تأكد أن `GoSumou` مسجل في لوحة MSEGAT |
| الرسالة لا تصل | تحقق من تنسيق الرقم (`966xxxxxxxxx` بدون `+`) |
| `Insufficient balance` | اشحن الرصيد عبر لوحة تحكم MSEGAT |
| OTP منتهي الصلاحية | أعد إرسال OTP جديد (المهلة الافتراضية: 5 دقائق) |

---

*آخر تحديث: 2026-04-19 | المشروع: Somou Al Thahabiya Land Transport*
