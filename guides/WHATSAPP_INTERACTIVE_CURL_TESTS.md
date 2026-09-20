# دليل اختبار الرسائل التفاعلية والشات بوت عبر cURL (WhatsApp Cloud API)
## تطبيق سمو الذهبية لنقل الركاب والليموزين الفاخر (VIP)

هذا الدليل يحتوي على جميع أوامر **cURL** المجهزة لمحاكاة واختبار القوائم التفاعلية، الفيديوهات التعليمية، أزرار الرد السريع، والتحويل لموظفي خدمة العملاء على رقم الهاتف: **`967713139235`**.

---

## 📑 الفهرس
1. [الرسالة الترحيبية (القائمة الرئيسية التفاعلية)](#1-الرسالة-الترحيبية-القائمة-الرئيسية-التفاعلية-)
2. [خيارات القائمة الرئيسية (List Reply Actions)](#2-خيارات-القائمة-الرئيسية-list-reply-actions)
   - [إنشاء وتفعيل الحساب 🆕](#1-إنشاء-وتفعيل-الحساب-)
   - [حجز رحلة جديدة VIP 🚗](#2-حجز-رحلة-جديدة-vip-)
   - [الانضمام ككابتن / شريك 👔](#3-الانضمام-ككابتن--شريك-)
   - [تتبع موقع السائق 📍](#4-تتبع-موقع-السائق-)
   - [تحميل التطبيق 📲](#5-تحميل-التطبيق-)
   - [الأسعار والمدفوعات 💳](#6-الأسعار-والمدفوعات-)
   - [تحدث مع موظف الدعم 👨‍💼](#7-تحدث-مع-موظف-الدعم-)
3. [الأزرار التفاعلية السريعة (Quick Reply Buttons)](#3-الأزرار-التفاعلية-السريعة-quick-reply-buttons)
   - [زر: نعم / تم بنجاح ✅](#8-زر-نعم--تم-بنجاح-)
   - [زر: لا - أحتاج مساعدة ❌](#9-زر-لا---أحتاج-مساعدة-)
   - [زر: القائمة الرئيسية 🏠](#10-زر-القائمة-الرئيسية-)

---

## 1. الرسالة الترحيبية (القائمة الرئيسية التفاعلية 📑)
> **الهدف:** إرسال رسالة نصية لبدء المحادثة وعرض القائمة التفاعلية الرئيسية المنبثقة فوراً.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_TEXT_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "text",
            "text": {"body": "السلام عليكم"}
          }]
        }
      }]
    }]
  }'
```

---

## 2. خيارات القائمة الرئيسية (List Reply Actions)

### 1) إنشاء وتفعيل الحساب 🆕 (`opt_create_account`)
> 🎬 **النتيجة:** يرسل فيديو شرح إنشاء الحساب + نص الخطوات وروابط التحميل + 3 أزرار متابعة.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_ACT_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "list_reply",
              "list_reply": { "id": "opt_create_account", "title": "إنشاء وتفعيل الحساب 🆕" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 2) حجز رحلة جديدة VIP 🚗 (`opt_book_ride`)
> 🎬 **النتيجة:** يرسل فيديو حجز الرحلات الفاخرة + خطوات تحديد المسار واختيار فئة السيارة + أزرار المتابعة.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_RIDE_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "list_reply",
              "list_reply": { "id": "opt_book_ride", "title": "حجز رحلة جديدة VIP 🚗" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 3) الانضمام ككابتن / شريك 👔 (`opt_partner_join`)
> 🎬 **النتيجة:** يرسل فيديو الكابتن + رابط بوابة تسجيل الشركاء + شروط الانضمام لأسطول سمو الذهبية.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_PART_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "list_reply",
              "list_reply": { "id": "opt_partner_join", "title": "الانضمام ككابتن / شريك 👔" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 4) تتبع موقع السائق 📍 (`opt_track_ride`)
> 🎬 **النتيجة:** يرسل فيديو التتبع المباشر + شرح متابعة مسار السائق لحظياً على الخريطة.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_TRK_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "list_reply",
              "list_reply": { "id": "opt_track_ride", "title": "تتبع موقع السائق 📍" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 5) تحميل التطبيق 📲 (`opt_download_app`)
> 📲 **النتيجة:** يرسل روابط تطبيق سمو الذهبية المباشرة لمتاجر iOS و Android.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_APP_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "list_reply",
              "list_reply": { "id": "opt_download_app", "title": "تحميل التطبيق 📲" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 6) الأسعار والمدفوعات 💳 (`opt_payments_info`)
> 💳 **النتيجة:** يرسل تفاصيل طرق الدفع (مدى، فيزا، ماستركارد، Apple Pay، سداد، الفواتير الضريبية).

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_PAY_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "list_reply",
              "list_reply": { "id": "opt_payments_info", "title": "الأسعار والمدفوعات 💳" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 7) تحدث مع موظف الدعم 👨‍💼 (`opt_live_support`)
> 👨‍💼 **النتيجة:** يرسل رسالة تحويل المحادثة لممثل خدمة العملاء البشري ويحدّث حالة التذكرة بالسيرفر.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_SUP_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "list_reply",
              "list_reply": { "id": "opt_live_support", "title": "تحدث مع موظف الدعم 👨‍💼" }
            }
          }]
        }
      }]
    }]
  }'
```

---

## 3. الأزرار التفاعلية السريعة (Quick Reply Buttons)

### 8) زر: "نعم / تم بنجاح ✅" (`btn_done_success`)
> 🌹 **النتيجة:** يرسل رسالة شكر وتمنيات برحلة سعيدة مع سمو الذهبية.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_BTN_DONE_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "button_reply",
              "button_reply": { "id": "btn_done_success", "title": "نعم ✅" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 9) زر: "لا - أحتاج مساعدة ❌" (`btn_need_help`)
> 👨‍💼 **النتيجة:** تحويل تلقائي للمحادثة إلى فريق خدمة العملاء والدعم الفوري.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_BTN_HELP_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "button_reply",
              "button_reply": { "id": "btn_need_help", "title": "لا - أحتاج مساعدة ❌" }
            }
          }]
        }
      }]
    }]
  }'
```

---

### 10) زر: "القائمة الرئيسية 🏠" (`btn_main_menu`)
> 📑 **النتيجة:** يعيد إرسال القائمة التفاعلية الرئيسية إلى العميل في أي وقت.

```bash
curl -X POST https://api.aldikka.com/api/v2/webhooks/whatsapp \
  -H "Content-Type: application/json" \
  -d '{
    "object": "whatsapp_business_account",
    "entry": [{
      "id": "0",
      "changes": [{
        "field": "messages",
        "value": {
          "messaging_product": "whatsapp",
          "contacts": [{"profile": {"name": "Protocol Soft"}, "wa_id": "967713139235"}],
          "messages": [{
            "id": "SIM_BTN_HOME_'"$(date +%s)"'",
            "timestamp": "'$(date +%s)'",
            "from": "967713139235",
            "type": "interactive",
            "interactive": {
              "type": "button_reply",
              "button_reply": { "id": "btn_main_menu", "title": "القائمة الرئيسية 🏠" }
            }
          }]
        }
      }]
    }]
  }'
```
