# توثيق واجهات برمجة التطبيقات (API Endpoints Documentation)

## جدول الملخص

| # | الموديول | طريقة الطلب (Method) | مسار الـ Endpoint | نشط / مهمل | الجداول المستهدفة |
|---|---|---|---|---|---|
| 1 | auth | POST | /admin/login | نشط | متعدد |
| 2 | auth | GET | /admin/profile | نشط | متعدد |
| 3 | auth | POST | /check-account-type | نشط | متعدد |
| 4 | auth | POST | /company/login/request-otp | نشط | متعدد |
| 5 | auth | POST | /company/login/verify-otp | نشط | متعدد |
| 6 | auth | POST | /partner/login/request-otp | نشط | متعدد |
| 7 | auth | POST | /partner/login/verify-otp | نشط | متعدد |
| 8 | auth | POST | /customer/login/request-otp | نشط | متعدد |
| 9 | auth | POST | /customer/login/verify-otp | نشط | متعدد |
| 10 | auth | POST | /alternative/check | نشط | متعدد |
| 11 | auth | POST | /alternative/request-otp | نشط | متعدد |
| 12 | auth | POST | /logout | نشط | متعدد |
| 13 | booking-templates | GET | / | نشط | متعدد |
| 14 | booking-templates | GET | /:templateId | نشط | متعدد |
| 15 | booking-templates | POST | / | نشط | متعدد |
| 16 | booking-templates | PUT | /:templateId | نشط | متعدد |
| 17 | booking-templates | DELETE | /:templateId | نشط | متعدد |
| 18 | booking-templates | POST | /:templateId/activate | نشط | متعدد |
| 19 | booking-templates | POST | /:templateId/deactivate | نشط | متعدد |
| 20 | bookings | GET | /admin/bookings | نشط | متعدد |
| 21 | bookings | GET | /admin/bookings/statistics | نشط | متعدد |
| 22 | bookings | GET | /templates | نشط | متعدد |
| 23 | bookings | POST | / | نشط | متعدد |
| 24 | bookings | POST | /guest | نشط | متعدد |
| 25 | bookings | GET | /:id | نشط | متعدد |
| 26 | bookings | POST | /:id/cancel | نشط | متعدد |
| 27 | bookings | GET | /:id/cancellation | نشط | متعدد |
| 28 | bookings | GET | /cancellations | نشط | متعدد |
| 29 | bookings | POST | /users/bookings | نشط | متعدد |
| 30 | bookings | PUT | /users/bookings/:id/payment | نشط | متعدد |
| 31 | bookings | PUT | /company/:id/payment | نشط | متعدد |
| 32 | companies | POST | / | نشط | متعدد |
| 33 | companies | GET | / | نشط | متعدد |
| 34 | companies | POST | /register | نشط | متعدد |
| 35 | companies | GET | /credit-alerts | نشط | متعدد |
| 36 | companies | GET | /:id | نشط | متعدد |
| 37 | companies | PUT | /:id | نشط | متعدد |
| 38 | companies | POST | /users | نشط | متعدد |
| 39 | company-credit | GET | /outstanding-balances | نشط | متعدد |
| 40 | company-credit | GET | /:companyId/outstanding-bookings | نشط | متعدد |
| 41 | company-credit | GET | /:companyId/collection-history | نشط | متعدد |
| 42 | company-credit | POST | /:companyId/collect-payment | نشط | متعدد |
| 43 | dashboard | GET | /stats | نشط | متعدد |
| 44 | dashboard | GET | /revenue | نشط | متعدد |
| 45 | fcm | POST | /register-token | نشط | متعدد |
| 46 | fcm | DELETE | /unregister-token | نشط | متعدد |
| 47 | fcm | PUT | /update-token | نشط | متعدد |
| 48 | fcm | POST | /test-notification | نشط | متعدد |
| 49 | guest | POST | /verify-phone | نشط | متعدد |
| 50 | invoice | GET | /my-company | نشط | متعدد |
| 51 | invoice | GET | /my-company/:id | نشط | متعدد |
| 52 | invoice | GET | /:id | نشط | متعدد |
| 53 | invoice | GET | / | نشط | متعدد |
| 54 | invoice | GET | /company/:id/stats | نشط | متعدد |
| 55 | invoice | POST | /auto-generate | نشط | متعدد |
| 56 | invoice | POST | /mark-overdue | نشط | متعدد |
| 57 | invoice | POST | /:id/send | نشط | متعدد |
| 58 | invoice | DELETE | /:id | نشط | متعدد |
| 59 | maps | POST | /distance | نشط | متعدد |
| 60 | maps | GET | /geocode | نشط | متعدد |
| 61 | maps | GET | /reverse-geocode | نشط | متعدد |
| 62 | maps | GET | /autocomplete | نشط | متعدد |
| 63 | maps | GET | /place-details | نشط | متعدد |
| 64 | master-data | GET | /regions | نشط | متعدد |
| 65 | master-data | GET | /services | نشط | متعدد |
| 66 | master-data | GET | /service-types | نشط | متعدد |
| 67 | master-data | GET | /vehicle-types | نشط | متعدد |
| 68 | operations | GET | /admin/available-partners | نشط | متعدد |
| 69 | operations | GET | /incoming | نشط | متعدد |
| 70 | operations | GET | /review/:bookingId | نشط | متعدد |
| 71 | operations | POST | /approve | نشط | متعدد |
| 72 | operations | POST | /reject | نشط | متعدد |
| 73 | operations | POST | /assign-driver | نشط | متعدد |
| 74 | operations | POST | /update-status | نشط | متعدد |
| 75 | otp | POST | /send | نشط | متعدد |
| 76 | otp | POST | /verify | نشط | متعدد |
| 77 | partners | POST | / | نشط | متعدد |
| 78 | partners | GET | / | نشط | متعدد |
| 79 | partners | GET | /me | نشط | متعدد |
| 80 | partners | DELETE | /me | نشط | متعدد |
| 81 | partners | GET | /:id | نشط | متعدد |
| 82 | partners | PUT | /:id | نشط | متعدد |
| 83 | partners | GET | /me | نشط | متعدد |
| 84 | payments | POST | /initiate | نشط | متعدد |
| 85 | payments | POST | /create-session | نشط | متعدد |
| 86 | payments | GET | /config | نشط | متعدد |
| 87 | payments | GET | /callback | نشط | متعدد |
| 88 | payments | POST | /callback | نشط | متعدد |
| 89 | payments | POST | /webhook/paytabs | نشط | متعدد |
| 90 | payments | POST | /webhook/paypal | نشط | متعدد |
| 91 | pricing | POST | /calculate | نشط | متعدد |
| 92 | pricing | GET | /effective | نشط | متعدد |
| 93 | rating | GET | /partners/:id/ratings | نشط | متعدد |
| 94 | rating | GET | /booking/:id | نشط | متعدد |
| 95 | rating | GET | /my-ratings | نشط | متعدد |
| 96 | rating | GET | /top-partners | نشط | متعدد |
| 97 | rating | DELETE | /:id | نشط | متعدد |
| 98 | sms | DELETE | /templates/:id | نشط | متعدد |
| 99 | sms | POST | /campaigns | نشط | متعدد |
| 100 | sms | POST | /campaigns/:id/send | نشط | متعدد |
| 101 | sms | POST | /campaigns/:id/cancel | نشط | متعدد |
| 102 | sms | DELETE | /campaigns/:id | نشط | متعدد |
| 103 | sms | POST | /segments/preview | نشط | متعدد |
| 104 | users | POST | / | نشط | متعدد |
| 105 | users | GET | /profile | نشط | متعدد |
| 106 | users | PUT | /profile | نشط | متعدد |
| 107 | users | DELETE | /me | نشط | متعدد |
| 108 | vehicles | POST | /types | نشط | متعدد |
| 109 | vehicles | GET | /types | نشط | متعدد |
| 110 | vehicles | POST | / | نشط | متعدد |
| 111 | vehicles | GET | /external/models | نشط | متعدد |
| 112 | whatsapp-webhook | POST | / | نشط | متعدد |
| 113 | whatsapp-webhook | GET | / | نشط | متعدد |
| 114 | withdrawals | GET | / | نشط | متعدد |
| 115 | withdrawals | GET | /stats | نشط | متعدد |
| 116 | withdrawals | GET | /:id | نشط | متعدد |
| 117 | withdrawals | PATCH | /:id/approve | نشط | متعدد |
| 118 | withdrawals | PATCH | /:id/reject | نشط | متعدد |
| 119 | withdrawals | PATCH | /:id/complete | نشط | متعدد |
| 120 | admin | GET | /bookings | نشط | متعدد |
| 121 | admin | PUT | /bookings/:id/status | نشط | متعدد |
| 122 | admin | GET | /bookings/:id/allowed-statuses | نشط | متعدد |
| 123 | admin | PUT | /bookings/:id/assign-partner | نشط | متعدد |
| 124 | admin | PUT | /bookings/:id/unassign-partner | نشط | متعدد |
| 125 | admin | GET | /bookings/statistics | نشط | متعدد |
| 126 | admin | GET | /bookings/archive | نشط | متعدد |
| 127 | admin | GET | /bookings/archive/statistics | نشط | متعدد |
| 128 | admin | GET | /bookings/archive/:id | نشط | متعدد |
| 129 | admin | POST | /bookings/:id/archive | نشط | متعدد |
| 130 | admin | POST | /bookings/archive/:id/restore | نشط | متعدد |
| 131 | admin | GET | /operations/available-partners | نشط | متعدد |
| 132 | admin | GET | /operations/ongoing | نشط | متعدد |
| 133 | admin | GET | /operations/internal-vehicles | نشط | متعدد |
| 134 | admin | GET | /operations/internal-drivers | نشط | متعدد |
| 135 | admin | GET | /operations/vehicles/:vehicleId/driver | نشط | متعدد |
| 136 | admin | GET | /operations/vehicles/:vehicleId/debug | نشط | متعدد |
| 137 | admin | GET | /operations/partners/:partnerId/debug | نشط | متعدد |
| 138 | admin | POST | /operations/:bookingId/assign-internal | نشط | متعدد |
| 139 | admin | GET | /users | نشط | متعدد |
| 140 | admin | POST | /users | نشط | متعدد |
| 141 | admin | GET | /users/:userId | نشط | متعدد |
| 142 | admin | PUT | /users/:userId | نشط | متعدد |
| 143 | admin | POST | /users/:userId/activate | نشط | متعدد |
| 144 | admin | POST | /users/:userId/deactivate | نشط | متعدد |
| 145 | admin | DELETE | /users/:userId | نشط | متعدد |
| 146 | admin | GET | /partners | نشط | متعدد |
| 147 | admin | GET | /partners/financial-transactions | نشط | متعدد |
| 148 | admin | GET | /partners/:partnerId | نشط | متعدد |
| 149 | admin | PUT | /partners/:partnerId | نشط | متعدد |
| 150 | admin | POST | /partners/:partnerId/activate | نشط | متعدد |
| 151 | admin | POST | /partners/:partnerId/deactivate | نشط | متعدد |
| 152 | admin | POST | /partners/:partnerId/approve | نشط | متعدد |
| 153 | admin | POST | /partners/:partnerId/reject | نشط | متعدد |
| 154 | admin | DELETE | /partners/:partnerId | نشط | متعدد |
| 155 | admin | PUT | /partners/:partnerId/commission | نشط | متعدد |
| 156 | admin | GET | /partners/:partnerId/vehicles | نشط | متعدد |
| 157 | admin | GET | /companies | نشط | متعدد |
| 158 | admin | GET | /companies/financial-transactions | نشط | متعدد |
| 159 | admin | GET | /companies/:companyId | نشط | متعدد |
| 160 | admin | GET | /companies/:companyId/users | نشط | متعدد |
| 161 | admin | PUT | /companies/:companyId | نشط | متعدد |
| 162 | admin | POST | /companies/internal/users | نشط | متعدد |
| 163 | admin | POST | /companies/:companyId/activate | نشط | متعدد |
| 164 | admin | POST | /companies/:companyId/deactivate | نشط | متعدد |
| 165 | admin | PUT | /company-users/:id | نشط | متعدد |
| 166 | admin | DELETE | /company-users/:id | نشط | متعدد |
| 167 | admin | PUT | /companies/:companyId/credit-limit | نشط | متعدد |
| 168 | admin | PUT | /companies/:companyId/discount | نشط | متعدد |
| 169 | admin | PUT | /companies/:companyId/commission-rate | نشط | متعدد |
| 170 | admin | POST | /companies/users/:userId/activate | نشط | متعدد |
| 171 | admin | POST | /companies/users/:userId/deactivate | نشط | متعدد |
| 172 | admin | PUT | /companies/users/:userId | نشط | متعدد |
| 173 | admin | DELETE | /companies/users/:userId | نشط | متعدد |
| 174 | admin | DELETE | /companies/:companyId | نشط | متعدد |
| 175 | admin | POST | /companies/:companyId/bookings | نشط | متعدد |
| 176 | admin | GET | /invoices | نشط | متعدد |
| 177 | admin | GET | /invoices/statistics | نشط | متعدد |
| 178 | admin | GET | /invoices/export | نشط | متعدد |
| 179 | admin | POST | /invoices/:id/send | نشط | متعدد |
| 180 | admin | POST | /invoices/:id/reminder | نشط | متعدد |
| 181 | admin | GET | /invoices/:id/delivery-history | نشط | متعدد |
| 182 | admin | DELETE | /invoices/:id | نشط | متعدد |
| 183 | admin | POST | /invoices/mark-overdue | نشط | متعدد |
| 184 | admin | POST | /invoices/auto-generate | نشط | متعدد |
| 185 | admin | GET | /invoices/uninvoiced-summary/:companyId | نشط | متعدد |
| 186 | admin | GET | /users/check-phone | نشط | متعدد |
| 187 | admin | GET | /users/check-email | نشط | متعدد |
| 188 | admin | GET | /vehicles | نشط | متعدد |
| 189 | admin | GET | /vehicles/:id | نشط | متعدد |
| 190 | admin | POST | /vehicles/:id/approve | نشط | متعدد |
| 191 | admin | POST | /vehicles/:id/reject | نشط | متعدد |
| 192 | admin | PUT | /vehicles/:id | نشط | متعدد |
| 193 | admin | DELETE | /vehicles/:id | نشط | متعدد |
| 194 | admin | GET | /settings/me/permissions | نشط | متعدد |
| 195 | admin | GET | /settings/me/permissions-v2 | نشط | متعدد |
| 196 | admin | GET | /settings/me/regions | نشط | متعدد |
| 197 | admin | GET | /settings/admins | نشط | متعدد |
| 198 | admin | POST | /settings/admins | نشط | متعدد |
| 199 | admin | GET | /settings/admins/:id/roles | نشط | متعدد |
| 200 | admin | POST | /settings/admins/:id/roles | نشط | متعدد |
| 201 | admin | DELETE | /settings/admins/:id/roles/:roleId | نشط | متعدد |
| 202 | admin | GET | /settings/admins/:id/regions | نشط | متعدد |
| 203 | admin | GET | /settings/admins/:id/permission-overrides | نشط | متعدد |
| 204 | admin | POST | /settings/admins/:id/permission-overrides | نشط | متعدد |
| 205 | admin | DELETE | /settings/admins/:id/permission-overrides/:overrideId | نشط | متعدد |
| 206 | admin | GET | /settings/admins/:id | نشط | متعدد |
| 207 | admin | GET | /settings/admins/:id/reports-count | نشط | متعدد |
| 208 | admin | PUT | /settings/admins/:id | نشط | متعدد |
| 209 | admin | DELETE | /settings/admins/:id | نشط | متعدد |
| 210 | admin | GET | /settings/permissions | نشط | متعدد |
| 211 | admin | POST | /settings/permissions | نشط | متعدد |
| 212 | admin | PUT | /settings/permissions/:id | نشط | متعدد |
| 213 | admin | GET | /settings/roles | نشط | متعدد |
| 214 | admin | POST | /settings/roles | نشط | متعدد |
| 215 | admin | GET | /settings/roles/:id/permissions | نشط | متعدد |
| 216 | admin | POST | /settings/roles/:id/permissions | نشط | متعدد |
| 217 | admin | DELETE | /settings/roles/:id/permissions/:rpId | نشط | متعدد |
| 218 | admin | GET | /settings/roles/:id | نشط | متعدد |
| 219 | admin | GET | /settings/roles/:id/admins-count | نشط | متعدد |
| 220 | admin | DELETE | /settings/roles/:id | نشط | متعدد |
| 221 | admin | PUT | /settings/roles/:id | نشط | متعدد |
| 222 | admin | GET | /settings/permissions-catalog | نشط | متعدد |
| 223 | admin | GET | /settings/roles/:id/permissions-v2 | نشط | متعدد |
| 224 | admin | PUT | /settings/roles/:id/permissions-v2 | نشط | متعدد |
| 225 | admin | GET | /settings/system | نشط | متعدد |
| 226 | admin | PUT | /settings/system | نشط | متعدد |
| 227 | admin | GET | /pricing | نشط | متعدد |
| 228 | admin | POST | /pricing | نشط | متعدد |
| 229 | admin | POST | /pricing/preview-conflicts | نشط | متعدد |
| 230 | admin | POST | /pricing/renew | نشط | متعدد |
| 231 | admin | POST | /pricing/bulk-delete | نشط | متعدد |
| 232 | admin | PUT | /pricing/:id | نشط | متعدد |
| 233 | admin | DELETE | /pricing/:id | نشط | متعدد |
| 234 | admin | GET | /master-data/services | نشط | متعدد |
| 235 | admin | POST | /master-data/services | نشط | متعدد |
| 236 | admin | PUT | /master-data/services/:id | نشط | متعدد |
| 237 | admin | DELETE | /master-data/services/:id | نشط | متعدد |
| 238 | admin | GET | /master-data/vehicle-types | نشط | متعدد |
| 239 | admin | POST | /master-data/vehicle-types | نشط | متعدد |
| 240 | admin | PUT | /master-data/vehicle-types/:id | نشط | متعدد |
| 241 | admin | DELETE | /master-data/vehicle-types/:id | نشط | متعدد |
| 242 | admin | GET | /master-data/regions | نشط | متعدد |
| 243 | admin | POST | /master-data/regions | نشط | متعدد |
| 244 | admin | PUT | /master-data/regions/:id | نشط | متعدد |
| 245 | admin | DELETE | /master-data/regions/:id | نشط | متعدد |
| 246 | admin | GET | /tracking/live | نشط | متعدد |
| 247 | admin | GET | /operations/ongoing | نشط | متعدد |
| 248 | admin | POST | /companies | نشط | متعدد |
| 249 | admin | GET | /booking-templates | نشط | متعدد |
| 250 | admin | GET | /booking-templates/:templateId | نشط | متعدد |
| 251 | admin | POST | /booking-templates | نشط | متعدد |
| 252 | admin | PUT | /booking-templates/:templateId | نشط | متعدد |
| 253 | admin | DELETE | /booking-templates/:templateId | نشط | متعدد |
| 254 | admin | POST | /booking-templates/:templateId/activate | نشط | متعدد |
| 255 | admin | POST | /booking-templates/:templateId/deactivate | نشط | متعدد |
| 256 | admin | GET | /notifications | نشط | متعدد |
| 257 | admin | POST | /notifications/mark-all-read | نشط | متعدد |
| 258 | index | GET | /health | نشط | متعدد |
| 259 | index | GET | /settings/public | نشط | متعدد |


## تفاصيل المسارات (Endpoints Details)

### 1. [POST] /admin/login
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 2. [GET] /admin/profile
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 3. [POST] /check-account-type
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 4. [POST] /company/login/request-otp
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 5. [POST] /company/login/verify-otp
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 6. [POST] /partner/login/request-otp
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 7. [POST] /partner/login/verify-otp
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 8. [POST] /customer/login/request-otp
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 9. [POST] /customer/login/verify-otp
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 10. [POST] /alternative/check
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 11. [POST] /alternative/request-otp
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 12. [POST] /logout
- **الموديول:** auth
- **الحالة:** نشط
- **ملف المسار:** [src/modules/auth/auth.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/auth/auth.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات auth

### 13. [GET] /
- **الموديول:** booking-templates
- **الحالة:** نشط
- **ملف المسار:** [src/modules/booking-templates/booking-templates.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/booking-templates/booking-templates.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات booking-templates

### 14. [GET] /:templateId
- **الموديول:** booking-templates
- **الحالة:** نشط
- **ملف المسار:** [src/modules/booking-templates/booking-templates.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/booking-templates/booking-templates.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات booking-templates

### 15. [POST] /
- **الموديول:** booking-templates
- **الحالة:** نشط
- **ملف المسار:** [src/modules/booking-templates/booking-templates.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/booking-templates/booking-templates.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات booking-templates

### 16. [PUT] /:templateId
- **الموديول:** booking-templates
- **الحالة:** نشط
- **ملف المسار:** [src/modules/booking-templates/booking-templates.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/booking-templates/booking-templates.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات booking-templates

### 17. [DELETE] /:templateId
- **الموديول:** booking-templates
- **الحالة:** نشط
- **ملف المسار:** [src/modules/booking-templates/booking-templates.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/booking-templates/booking-templates.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات booking-templates

### 18. [POST] /:templateId/activate
- **الموديول:** booking-templates
- **الحالة:** نشط
- **ملف المسار:** [src/modules/booking-templates/booking-templates.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/booking-templates/booking-templates.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات booking-templates

### 19. [POST] /:templateId/deactivate
- **الموديول:** booking-templates
- **الحالة:** نشط
- **ملف المسار:** [src/modules/booking-templates/booking-templates.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/booking-templates/booking-templates.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات booking-templates

### 20. [GET] /admin/bookings
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 21. [GET] /admin/bookings/statistics
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 22. [GET] /templates
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 23. [POST] /
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 24. [POST] /guest
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 25. [GET] /:id
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 26. [POST] /:id/cancel
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 27. [GET] /:id/cancellation
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 28. [GET] /cancellations
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 29. [POST] /users/bookings
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 30. [PUT] /users/bookings/:id/payment
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 31. [PUT] /company/:id/payment
- **الموديول:** bookings
- **الحالة:** نشط
- **ملف المسار:** [src/modules/bookings/bookings.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/bookings/bookings.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات bookings

### 32. [POST] /
- **الموديول:** companies
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/companies.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/companies.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات companies

### 33. [GET] /
- **الموديول:** companies
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/companies.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/companies.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات companies

### 34. [POST] /register
- **الموديول:** companies
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/companies.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/companies.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات companies

### 35. [GET] /credit-alerts
- **الموديول:** companies
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/companies.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/companies.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات companies

### 36. [GET] /:id
- **الموديول:** companies
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/companies.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/companies.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات companies

### 37. [PUT] /:id
- **الموديول:** companies
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/companies.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/companies.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات companies

### 38. [POST] /users
- **الموديول:** companies
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/companies.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/companies.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات companies

### 39. [GET] /outstanding-balances
- **الموديول:** company-credit
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/company-credit.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/company-credit.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات company-credit

### 40. [GET] /:companyId/outstanding-bookings
- **الموديول:** company-credit
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/company-credit.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/company-credit.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات company-credit

### 41. [GET] /:companyId/collection-history
- **الموديول:** company-credit
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/company-credit.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/company-credit.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات company-credit

### 42. [POST] /:companyId/collect-payment
- **الموديول:** company-credit
- **الحالة:** نشط
- **ملف المسار:** [src/modules/companies/company-credit.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/companies/company-credit.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات company-credit

### 43. [GET] /stats
- **الموديول:** dashboard
- **الحالة:** نشط
- **ملف المسار:** [src/modules/dashboard/dashboard.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/dashboard/dashboard.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات dashboard

### 44. [GET] /revenue
- **الموديول:** dashboard
- **الحالة:** نشط
- **ملف المسار:** [src/modules/dashboard/dashboard.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/dashboard/dashboard.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات dashboard

### 45. [POST] /register-token
- **الموديول:** fcm
- **الحالة:** نشط
- **ملف المسار:** [src/modules/fcm/fcm.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/fcm/fcm.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات fcm

### 46. [DELETE] /unregister-token
- **الموديول:** fcm
- **الحالة:** نشط
- **ملف المسار:** [src/modules/fcm/fcm.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/fcm/fcm.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات fcm

### 47. [PUT] /update-token
- **الموديول:** fcm
- **الحالة:** نشط
- **ملف المسار:** [src/modules/fcm/fcm.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/fcm/fcm.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات fcm

### 48. [POST] /test-notification
- **الموديول:** fcm
- **الحالة:** نشط
- **ملف المسار:** [src/modules/fcm/fcm.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/fcm/fcm.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات fcm

### 49. [POST] /verify-phone
- **الموديول:** guest
- **الحالة:** نشط
- **ملف المسار:** [src/modules/guest/guest.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/guest/guest.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات guest

### 50. [GET] /my-company
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 51. [GET] /my-company/:id
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 52. [GET] /:id
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 53. [GET] /
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 54. [GET] /company/:id/stats
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 55. [POST] /auto-generate
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 56. [POST] /mark-overdue
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 57. [POST] /:id/send
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 58. [DELETE] /:id
- **الموديول:** invoice
- **الحالة:** نشط
- **ملف المسار:** [src/modules/invoices/invoice.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/invoices/invoice.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات invoice

### 59. [POST] /distance
- **الموديول:** maps
- **الحالة:** نشط
- **ملف المسار:** [src/modules/maps/maps.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/maps/maps.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات maps

### 60. [GET] /geocode
- **الموديول:** maps
- **الحالة:** نشط
- **ملف المسار:** [src/modules/maps/maps.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/maps/maps.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات maps

### 61. [GET] /reverse-geocode
- **الموديول:** maps
- **الحالة:** نشط
- **ملف المسار:** [src/modules/maps/maps.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/maps/maps.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات maps

### 62. [GET] /autocomplete
- **الموديول:** maps
- **الحالة:** نشط
- **ملف المسار:** [src/modules/maps/maps.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/maps/maps.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات maps

### 63. [GET] /place-details
- **الموديول:** maps
- **الحالة:** نشط
- **ملف المسار:** [src/modules/maps/maps.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/maps/maps.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات maps

### 64. [GET] /regions
- **الموديول:** master-data
- **الحالة:** نشط
- **ملف المسار:** [src/modules/master-data/master-data.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/master-data/master-data.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات master-data

### 65. [GET] /services
- **الموديول:** master-data
- **الحالة:** نشط
- **ملف المسار:** [src/modules/master-data/master-data.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/master-data/master-data.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات master-data

### 66. [GET] /service-types
- **الموديول:** master-data
- **الحالة:** نشط
- **ملف المسار:** [src/modules/master-data/master-data.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/master-data/master-data.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات master-data

### 67. [GET] /vehicle-types
- **الموديول:** master-data
- **الحالة:** نشط
- **ملف المسار:** [src/modules/master-data/master-data.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/master-data/master-data.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات master-data

### 68. [GET] /admin/available-partners
- **الموديول:** operations
- **الحالة:** نشط
- **ملف المسار:** [src/modules/operations/operations.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/operations/operations.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات operations

### 69. [GET] /incoming
- **الموديول:** operations
- **الحالة:** نشط
- **ملف المسار:** [src/modules/operations/operations.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/operations/operations.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات operations

### 70. [GET] /review/:bookingId
- **الموديول:** operations
- **الحالة:** نشط
- **ملف المسار:** [src/modules/operations/operations.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/operations/operations.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات operations

### 71. [POST] /approve
- **الموديول:** operations
- **الحالة:** نشط
- **ملف المسار:** [src/modules/operations/operations.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/operations/operations.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات operations

### 72. [POST] /reject
- **الموديول:** operations
- **الحالة:** نشط
- **ملف المسار:** [src/modules/operations/operations.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/operations/operations.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات operations

### 73. [POST] /assign-driver
- **الموديول:** operations
- **الحالة:** نشط
- **ملف المسار:** [src/modules/operations/operations.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/operations/operations.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات operations

### 74. [POST] /update-status
- **الموديول:** operations
- **الحالة:** نشط
- **ملف المسار:** [src/modules/operations/operations.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/operations/operations.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات operations

### 75. [POST] /send
- **الموديول:** otp
- **الحالة:** نشط
- **ملف المسار:** [src/modules/otp/otp.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/otp/otp.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات otp

### 76. [POST] /verify
- **الموديول:** otp
- **الحالة:** نشط
- **ملف المسار:** [src/modules/otp/otp.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/otp/otp.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات otp

### 77. [POST] /
- **الموديول:** partners
- **الحالة:** نشط
- **ملف المسار:** [src/modules/partners/partners.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/partners/partners.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات partners

### 78. [GET] /
- **الموديول:** partners
- **الحالة:** نشط
- **ملف المسار:** [src/modules/partners/partners.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/partners/partners.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات partners

### 79. [GET] /me
- **الموديول:** partners
- **الحالة:** نشط
- **ملف المسار:** [src/modules/partners/partners.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/partners/partners.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات partners

### 80. [DELETE] /me
- **الموديول:** partners
- **الحالة:** نشط
- **ملف المسار:** [src/modules/partners/partners.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/partners/partners.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات partners

### 81. [GET] /:id
- **الموديول:** partners
- **الحالة:** نشط
- **ملف المسار:** [src/modules/partners/partners.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/partners/partners.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات partners

### 82. [PUT] /:id
- **الموديول:** partners
- **الحالة:** نشط
- **ملف المسار:** [src/modules/partners/partners.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/partners/partners.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات partners

### 83. [GET] /me
- **الموديول:** partners
- **الحالة:** نشط
- **ملف المسار:** [src/modules/partners/partners.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/partners/partners.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات partners

### 84. [POST] /initiate
- **الموديول:** payments
- **الحالة:** نشط
- **ملف المسار:** [src/modules/payments/payments.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/payments/payments.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات payments

### 85. [POST] /create-session
- **الموديول:** payments
- **الحالة:** نشط
- **ملف المسار:** [src/modules/payments/payments.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/payments/payments.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات payments

### 86. [GET] /config
- **الموديول:** payments
- **الحالة:** نشط
- **ملف المسار:** [src/modules/payments/payments.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/payments/payments.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات payments

### 87. [GET] /callback
- **الموديول:** payments
- **الحالة:** نشط
- **ملف المسار:** [src/modules/payments/payments.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/payments/payments.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات payments

### 88. [POST] /callback
- **الموديول:** payments
- **الحالة:** نشط
- **ملف المسار:** [src/modules/payments/payments.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/payments/payments.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات payments

### 89. [POST] /webhook/paytabs
- **الموديول:** payments
- **الحالة:** نشط
- **ملف المسار:** [src/modules/payments/payments.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/payments/payments.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات payments

### 90. [POST] /webhook/paypal
- **الموديول:** payments
- **الحالة:** نشط
- **ملف المسار:** [src/modules/payments/payments.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/payments/payments.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات payments

### 91. [POST] /calculate
- **الموديول:** pricing
- **الحالة:** نشط
- **ملف المسار:** [src/modules/pricing/pricing.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/pricing/pricing.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات pricing

### 92. [GET] /effective
- **الموديول:** pricing
- **الحالة:** نشط
- **ملف المسار:** [src/modules/pricing/pricing.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/pricing/pricing.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات pricing

### 93. [GET] /partners/:id/ratings
- **الموديول:** rating
- **الحالة:** نشط
- **ملف المسار:** [src/modules/ratings/rating.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/ratings/rating.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات rating

### 94. [GET] /booking/:id
- **الموديول:** rating
- **الحالة:** نشط
- **ملف المسار:** [src/modules/ratings/rating.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/ratings/rating.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات rating

### 95. [GET] /my-ratings
- **الموديول:** rating
- **الحالة:** نشط
- **ملف المسار:** [src/modules/ratings/rating.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/ratings/rating.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات rating

### 96. [GET] /top-partners
- **الموديول:** rating
- **الحالة:** نشط
- **ملف المسار:** [src/modules/ratings/rating.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/ratings/rating.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات rating

### 97. [DELETE] /:id
- **الموديول:** rating
- **الحالة:** نشط
- **ملف المسار:** [src/modules/ratings/rating.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/ratings/rating.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات rating

### 98. [DELETE] /templates/:id
- **الموديول:** sms
- **الحالة:** نشط
- **ملف المسار:** [src/modules/sms/sms.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/sms/sms.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات sms

### 99. [POST] /campaigns
- **الموديول:** sms
- **الحالة:** نشط
- **ملف المسار:** [src/modules/sms/sms.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/sms/sms.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات sms

### 100. [POST] /campaigns/:id/send
- **الموديول:** sms
- **الحالة:** نشط
- **ملف المسار:** [src/modules/sms/sms.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/sms/sms.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات sms

### 101. [POST] /campaigns/:id/cancel
- **الموديول:** sms
- **الحالة:** نشط
- **ملف المسار:** [src/modules/sms/sms.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/sms/sms.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات sms

### 102. [DELETE] /campaigns/:id
- **الموديول:** sms
- **الحالة:** نشط
- **ملف المسار:** [src/modules/sms/sms.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/sms/sms.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات sms

### 103. [POST] /segments/preview
- **الموديول:** sms
- **الحالة:** نشط
- **ملف المسار:** [src/modules/sms/sms.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/sms/sms.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات sms

### 104. [POST] /
- **الموديول:** users
- **الحالة:** نشط
- **ملف المسار:** [src/modules/users/users.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/users/users.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات users

### 105. [GET] /profile
- **الموديول:** users
- **الحالة:** نشط
- **ملف المسار:** [src/modules/users/users.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/users/users.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات users

### 106. [PUT] /profile
- **الموديول:** users
- **الحالة:** نشط
- **ملف المسار:** [src/modules/users/users.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/users/users.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات users

### 107. [DELETE] /me
- **الموديول:** users
- **الحالة:** نشط
- **ملف المسار:** [src/modules/users/users.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/users/users.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات users

### 108. [POST] /types
- **الموديول:** vehicles
- **الحالة:** نشط
- **ملف المسار:** [src/modules/vehicles/vehicles.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/vehicles/vehicles.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات vehicles

### 109. [GET] /types
- **الموديول:** vehicles
- **الحالة:** نشط
- **ملف المسار:** [src/modules/vehicles/vehicles.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/vehicles/vehicles.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات vehicles

### 110. [POST] /
- **الموديول:** vehicles
- **الحالة:** نشط
- **ملف المسار:** [src/modules/vehicles/vehicles.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/vehicles/vehicles.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات vehicles

### 111. [GET] /external/models
- **الموديول:** vehicles
- **الحالة:** نشط
- **ملف المسار:** [src/modules/vehicles/vehicles.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/vehicles/vehicles.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات vehicles

### 112. [POST] /
- **الموديول:** whatsapp-webhook
- **الحالة:** نشط
- **ملف المسار:** [src/modules/whatsapp/whatsapp-webhook.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/whatsapp/whatsapp-webhook.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات whatsapp-webhook

### 113. [GET] /
- **الموديول:** whatsapp-webhook
- **الحالة:** نشط
- **ملف المسار:** [src/modules/whatsapp/whatsapp-webhook.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/whatsapp/whatsapp-webhook.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات whatsapp-webhook

### 114. [GET] /
- **الموديول:** withdrawals
- **الحالة:** نشط
- **ملف المسار:** [src/modules/withdrawals/withdrawals.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/withdrawals/withdrawals.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات withdrawals

### 115. [GET] /stats
- **الموديول:** withdrawals
- **الحالة:** نشط
- **ملف المسار:** [src/modules/withdrawals/withdrawals.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/withdrawals/withdrawals.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات withdrawals

### 116. [GET] /:id
- **الموديول:** withdrawals
- **الحالة:** نشط
- **ملف المسار:** [src/modules/withdrawals/withdrawals.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/withdrawals/withdrawals.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات withdrawals

### 117. [PATCH] /:id/approve
- **الموديول:** withdrawals
- **الحالة:** نشط
- **ملف المسار:** [src/modules/withdrawals/withdrawals.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/withdrawals/withdrawals.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات withdrawals

### 118. [PATCH] /:id/reject
- **الموديول:** withdrawals
- **الحالة:** نشط
- **ملف المسار:** [src/modules/withdrawals/withdrawals.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/withdrawals/withdrawals.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات withdrawals

### 119. [PATCH] /:id/complete
- **الموديول:** withdrawals
- **الحالة:** نشط
- **ملف المسار:** [src/modules/withdrawals/withdrawals.routes.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/modules/withdrawals/withdrawals.routes.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات withdrawals

### 120. [GET] /bookings
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 121. [PUT] /bookings/:id/status
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 122. [GET] /bookings/:id/allowed-statuses
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 123. [PUT] /bookings/:id/assign-partner
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 124. [PUT] /bookings/:id/unassign-partner
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 125. [GET] /bookings/statistics
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 126. [GET] /bookings/archive
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 127. [GET] /bookings/archive/statistics
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 128. [GET] /bookings/archive/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 129. [POST] /bookings/:id/archive
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 130. [POST] /bookings/archive/:id/restore
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 131. [GET] /operations/available-partners
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 132. [GET] /operations/ongoing
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 133. [GET] /operations/internal-vehicles
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 134. [GET] /operations/internal-drivers
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 135. [GET] /operations/vehicles/:vehicleId/driver
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 136. [GET] /operations/vehicles/:vehicleId/debug
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 137. [GET] /operations/partners/:partnerId/debug
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 138. [POST] /operations/:bookingId/assign-internal
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 139. [GET] /users
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 140. [POST] /users
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 141. [GET] /users/:userId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 142. [PUT] /users/:userId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 143. [POST] /users/:userId/activate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 144. [POST] /users/:userId/deactivate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 145. [DELETE] /users/:userId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 146. [GET] /partners
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 147. [GET] /partners/financial-transactions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 148. [GET] /partners/:partnerId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 149. [PUT] /partners/:partnerId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 150. [POST] /partners/:partnerId/activate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 151. [POST] /partners/:partnerId/deactivate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 152. [POST] /partners/:partnerId/approve
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 153. [POST] /partners/:partnerId/reject
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 154. [DELETE] /partners/:partnerId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 155. [PUT] /partners/:partnerId/commission
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 156. [GET] /partners/:partnerId/vehicles
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 157. [GET] /companies
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 158. [GET] /companies/financial-transactions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 159. [GET] /companies/:companyId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 160. [GET] /companies/:companyId/users
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 161. [PUT] /companies/:companyId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 162. [POST] /companies/internal/users
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 163. [POST] /companies/:companyId/activate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 164. [POST] /companies/:companyId/deactivate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 165. [PUT] /company-users/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 166. [DELETE] /company-users/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 167. [PUT] /companies/:companyId/credit-limit
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 168. [PUT] /companies/:companyId/discount
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 169. [PUT] /companies/:companyId/commission-rate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 170. [POST] /companies/users/:userId/activate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 171. [POST] /companies/users/:userId/deactivate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 172. [PUT] /companies/users/:userId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 173. [DELETE] /companies/users/:userId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 174. [DELETE] /companies/:companyId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 175. [POST] /companies/:companyId/bookings
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 176. [GET] /invoices
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 177. [GET] /invoices/statistics
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 178. [GET] /invoices/export
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 179. [POST] /invoices/:id/send
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 180. [POST] /invoices/:id/reminder
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 181. [GET] /invoices/:id/delivery-history
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 182. [DELETE] /invoices/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 183. [POST] /invoices/mark-overdue
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 184. [POST] /invoices/auto-generate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 185. [GET] /invoices/uninvoiced-summary/:companyId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 186. [GET] /users/check-phone
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 187. [GET] /users/check-email
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 188. [GET] /vehicles
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 189. [GET] /vehicles/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 190. [POST] /vehicles/:id/approve
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 191. [POST] /vehicles/:id/reject
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 192. [PUT] /vehicles/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 193. [DELETE] /vehicles/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 194. [GET] /settings/me/permissions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 195. [GET] /settings/me/permissions-v2
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 196. [GET] /settings/me/regions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 197. [GET] /settings/admins
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 198. [POST] /settings/admins
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 199. [GET] /settings/admins/:id/roles
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 200. [POST] /settings/admins/:id/roles
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 201. [DELETE] /settings/admins/:id/roles/:roleId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 202. [GET] /settings/admins/:id/regions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 203. [GET] /settings/admins/:id/permission-overrides
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 204. [POST] /settings/admins/:id/permission-overrides
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 205. [DELETE] /settings/admins/:id/permission-overrides/:overrideId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 206. [GET] /settings/admins/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 207. [GET] /settings/admins/:id/reports-count
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 208. [PUT] /settings/admins/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 209. [DELETE] /settings/admins/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 210. [GET] /settings/permissions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 211. [POST] /settings/permissions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 212. [PUT] /settings/permissions/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 213. [GET] /settings/roles
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 214. [POST] /settings/roles
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 215. [GET] /settings/roles/:id/permissions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 216. [POST] /settings/roles/:id/permissions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 217. [DELETE] /settings/roles/:id/permissions/:rpId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 218. [GET] /settings/roles/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 219. [GET] /settings/roles/:id/admins-count
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 220. [DELETE] /settings/roles/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 221. [PUT] /settings/roles/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 222. [GET] /settings/permissions-catalog
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 223. [GET] /settings/roles/:id/permissions-v2
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 224. [PUT] /settings/roles/:id/permissions-v2
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 225. [GET] /settings/system
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 226. [PUT] /settings/system
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 227. [GET] /pricing
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 228. [POST] /pricing
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 229. [POST] /pricing/preview-conflicts
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 230. [POST] /pricing/renew
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 231. [POST] /pricing/bulk-delete
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 232. [PUT] /pricing/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 233. [DELETE] /pricing/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 234. [GET] /master-data/services
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 235. [POST] /master-data/services
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 236. [PUT] /master-data/services/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 237. [DELETE] /master-data/services/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 238. [GET] /master-data/vehicle-types
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 239. [POST] /master-data/vehicle-types
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 240. [PUT] /master-data/vehicle-types/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 241. [DELETE] /master-data/vehicle-types/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 242. [GET] /master-data/regions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 243. [POST] /master-data/regions
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 244. [PUT] /master-data/regions/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 245. [DELETE] /master-data/regions/:id
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 246. [GET] /tracking/live
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 247. [GET] /operations/ongoing
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 248. [POST] /companies
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 249. [GET] /booking-templates
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 250. [GET] /booking-templates/:templateId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 251. [POST] /booking-templates
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 252. [PUT] /booking-templates/:templateId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 253. [DELETE] /booking-templates/:templateId
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 254. [POST] /booking-templates/:templateId/activate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 255. [POST] /booking-templates/:templateId/deactivate
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 256. [GET] /notifications
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 257. [POST] /notifications/mark-all-read
- **الموديول:** admin
- **الحالة:** نشط
- **ملف المسار:** [src/routes/admin.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/admin.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات admin

### 258. [GET] /health
- **الموديول:** index
- **الحالة:** نشط
- **ملف المسار:** [src/routes/index.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/index.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات index

### 259. [GET] /settings/public
- **الموديول:** index
- **الحالة:** نشط
- **ملف المسار:** [src/routes/index.ts](file:///c:/Samu/production/somou-al-thahabiya/somou-al-thahabiya-server/src/routes/index.ts)
- **Middleware & Security:** Auth Required / Role Required (راجع الكود)
- **Request Specifications:** يرجى مراجعة ملف الـ schema والكنترولر
- **Response Specifications:** استجابة JSON قياسية 200/400/500
- **Database Execution:** استعلامات قاعدة بيانات index

