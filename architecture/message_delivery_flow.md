# تدفق إرسال الرسائل الخارجية (WhatsApp 360dialog)

## الحالة الحالية

- **القناة الوحيدة المفعّلة** لإرسال الرسائل النصية الخارجية إلى العملاء والسائقين والشركاء هي **WhatsApp عبر 360dialog** (`https://waba-v2.360dialog.io/messages`) باستخدام **قوالب Meta المعتمدة** (template messages).
- **SMS و Twilio** معطّلان مؤقتًا: دوال مثل `sendSms` / `sendSmsTwilio` / `sendWhatsappTwilio` لا ترسل عبر Twilio؛ واستدعاء `sendOtpSms` من `OtpService` معلّق مع TODO حتى يتوفر مزود SMS أو قالب OTP معتمد.
- **لم يُغيّر** هذا المستند أو الكود المرتبط به: البريد (SendGrid)، إشعارات قاعدة البيانات، FCM، Socket.IO.

## الإعداد

| المتغير | الوصف |
|--------|--------|
| `D360_API_KEY` | مفتاح API لـ 360dialog (رأس `D360-API-KEY`) |
| `D360_MESSAGES_URL` | اختياري؛ الافتراضي `https://waba-v2.360dialog.io/messages` |

قائمة حالة اعتماد القوالب في الكود: `src/whatsapp/whatsapp-templates.config.ts` (`WHATSAPP_TEMPLATE_APPROVAL`).

## طبقة الإرسال

- **`sendWhatsappTemplate`**: يبني `type: template` ويرسل إلى 360dialog.
- **`sendWhatsappTemplateWithFallback`**: إذا كانت القالب المطلوبة **pending** في الخريطة، يُرسل مباشرة عبر **`general_notification_vip`** (قالب واحد `{{1}}` بنص كامل مُصفّى). إذا كانت **approved** وفشل الطلب، يُعاد المحاولة بنفس القالب البديل.

قيود نص المعاملات: لا أسطر جديدة ولا تابات ولا أكثر من 4 مسافات متتالية داخل قيمة `text` — تُطبَّق عبر `sanitizeTemplateParameterText`.

## ربط القوالب بأنواع الرسائل (ملخص)

| القالب | الاستخدام في الكود |
|--------|---------------------|
| `booking_confirmation_vip` | تأكيد الحجز (إداري → CONFIRMED)، و`sendPassengerTripDetailsSms` عند `status === 'CONFIRMED'` |
| `booking_status_update_vip_1` | تحديثات الحالة التشغيلية، إلغاء، استلام طلب (NEW)، تفاصيل السلة (رسالة لكل حجز)، تعيين سائق للعميل (نص مختصر) |
| `trip_reminder_vip` | `TRIP_REMINDER` / تذكير قبل الرحلة |
| `driver_assignment_vip` | تعيين حجز للسائق؛ تعيين من لوحة الإدارة |
| `driver_vehicle_details` | قبول السائق للعميل؛ «السائق في الطريق» عند توفر بيانات المركبة/الهاتف من التتبع |
| `partner_finance_update_vip_1` | إشعارات الشركاء المالية داخل `createNotification` (WITHDRAW_* وما شابه) |
| `general_notification_vip` | بديل عند قالب pending أو فشل الإرسال؛ وأي مسار legacy عبر `sendWhatsapp(to, body)` |

## السلة متعددة الحجوزات

`sendMultipleBookingsSms` يرسل **رسالة WhatsApp منفصلة لكل حجز** في السلة (نفس القالب المناسب لكل عنصر، حاليًا `booking_status_update_vip_1` مع fallback).

## الملفات المرجعية

- `src/whatsapp/whatsapp-360dialog.service.ts` — بناء الطلب والـ fallback
- `src/whatsapp/whatsapp-templates.config.ts` — أسماء القوالب وحالة الاعتماد
- `src/modules/notifications/notifications.service.ts` — دوال المحتوى لكل سيناريو
