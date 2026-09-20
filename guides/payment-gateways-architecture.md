# Payment Module - Dynamic Gateway Architecture

## Overview

بنية دفع ديناميكية تدعم **PayTabs** و **PayPal** مع تكامل Client-Side (iframe / SDK)، يتم اختيار البوابة عبر متغير البيئة:

```
PAYMENT_GATEWAY=paytabs   # أو
PAYMENT_GATEWAY=paypal
```

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  PaymentsService / BookingsService                              │
│  استدعاء: gateway.createSession(), gateway.handleWebhook()      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  Gateway Factory (gateway.factory.ts)                           │
│  يعيد: PayTabsAdapter أو PayPalAdapter حسب PAYMENT_GATEWAY      │
└─────────────────────────────────────────────────────────────────┘
              │                                    │
              ▼                                    ▼
    ┌──────────────────┐                 ┌──────────────────┐
    │ PayTabsAdapter   │                 │ PayPalAdapter    │
    │ (iframe/redirect)│                 │ (JS SDK buttons) │
    └──────────────────┘                 └──────────────────┘
```

## File Structure

```
payments/
├── gateways/
│   ├── types.ts          # IPaymentGateway, CreateSessionInput, WebhookResult
│   ├── paytabs.adapter.ts
│   ├── paypal.adapter.ts
│   └── gateway.factory.ts
├── payments.service.ts   # Uses getPaymentGateway()
├── payments.controller.ts
├── payments.routes.ts
└── README.md

external/
├── paytabs.client.ts     # + framed option for iframe
└── paypal.client.ts      # create order, capture
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| POST | `/payments/create-session` | إنشاء جلسة دفع (يعيد payment_url أو order_id حسب البوابة) |
| GET | `/payments/config` | إعدادات العميل (gateway, clientId/clientKey) للـ frontend |
| GET/POST | `/payments/callback` | نقطة عودة موحدة (PayTabs + PayPal) |
| POST | `/payments/webhook/paytabs` | Webhook PayTabs |
| POST | `/payments/webhook/paypal` | Webhook PayPal |

## Environment Variables

```env
# Gateway selection
PAYMENT_GATEWAY=paytabs

# PayTabs (when PAYMENT_GATEWAY=paytabs)
PAYTABS_PROFILE_ID=
PAYTABS_SERVER_KEY=
PAYTABS_CLIENT_KEY=
PAYTABS_CURRENCY=SAR

# PayPal (when PAYMENT_GATEWAY=paypal)
PAYPAL_CLIENT_ID=
PAYPAL_CLIENT_SECRET=
PAYPAL_CURRENCY=USD
PAYPAL_SANDBOX=true
PAYPAL_BRAND_NAME=Somou Al Thahabiya
```

## Frontend Integration

المكون `PaymentCheckout` في `frontend-marketing/components/payment/PaymentCheckout.tsx`:

1. يستدعي `GET /payments/config` لمعرفة البوابة
2. يستدعي `POST /payments/create-session` لإنشاء الجلسة
3. حسب البوابة:
   - **PayTabs**: iframe (useFramed) أو redirect
   - **PayPal**: أزرار PayPal JS SDK مع order_id

## Client-Side Flow

### PayTabs (iframe)
- السيرفر يرسل `framed: true` لـ PayTabs
- يرجع `redirect_url` للصفحة المُضمَّنة
- الـ frontend يعرضها في `<iframe>`

### PayPal
- السيرفر ينشئ order عبر Orders API
- يرجع `order_id`
- الـ frontend يحمل PayPal SDK ويعرض أزرار الدفع
- عند الموافقة، PayPal يعيد توجيه المستخدم إلى `/payments/callback?gateway=paypal&token=ORDER_ID`
- الـ callback يستدعي capture ويحدّث قاعدة البيانات

## Switching Gateways

لتغيير البوابة، عدّل `.env`:

```env
PAYMENT_GATEWAY=paypal
PAYPAL_CLIENT_ID=...
PAYPAL_CLIENT_SECRET=...
```

ولا حاجة لتعديل أي كود إضافي.
