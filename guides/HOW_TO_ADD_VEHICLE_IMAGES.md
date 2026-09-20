# كيفية إضافة صور السيارات في قاعدة البيانات

## نظرة عامة

صور السيارات يتم تخزينها في جدول `vehicle_types` وليس في جدول `vehicles` المباشر. هذا يعني أن كل **نوع سيارة** (مثل: سيدان، SUV، فان، إلخ) له صورة واحدة تمثله.

## بنية قاعدة البيانات

### جدول `vehicle_types`

\`\`\`sql
CREATE TABLE vehicle_types (
  vehicle_type_id SERIAL PRIMARY KEY,
  type_name VARCHAR(100) NOT NULL,
  type_name_en VARCHAR(100),
  description TEXT,
  image_url VARCHAR(500),  -- رابط صورة نوع السيارة
  sort_order INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);
\`\`\`

### جدول `vehicles`

\`\`\`sql
CREATE TABLE vehicles (
  vehicle_id SERIAL PRIMARY KEY,
  partner_id INTEGER REFERENCES partners(partner_id),
  vehicle_type_id INTEGER REFERENCES vehicle_types(vehicle_type_id),
  plate_number VARCHAR(50) NOT NULL,
  brand VARCHAR(100),
  model VARCHAR(100),
  manufacture_year INTEGER,
  color VARCHAR(50),
  seats INTEGER DEFAULT 4,
  is_active BOOLEAN DEFAULT TRUE,
  registration_file VARCHAR(500),
  insurance_file VARCHAR(500),
  created_at TIMESTAMP DEFAULT NOW()
);
\`\`\`

## كيفية إضافة الصور

### 1. عبر API

يمكنك إضافة أو تحديث صورة نوع السيارة عبر API:

\`\`\`bash
# إنشاء نوع سيارة جديد مع صورة
POST /api/admin/vehicle-types
Content-Type: application/json

{
  "type_name": "سيدان",
  "type_name_en": "Sedan",
  "description": "سيارة سيدان عادية",
  "image_url": "https://example.com/images/sedan.jpg",
  "sort_order": 1
}
\`\`\`

\`\`\`bash
# تحديث صورة نوع سيارة موجود
PUT /api/admin/vehicle-types/:id
Content-Type: application/json

{
  "image_url": "https://example.com/images/new-sedan.jpg"
}
\`\`\`

### 2. عبر قاعدة البيانات مباشرة

\`\`\`sql
-- إضافة صورة لنوع سيارة موجود
UPDATE vehicle_types
SET image_url = 'https://example.com/images/sedan.jpg'
WHERE vehicle_type_id = 1;

-- إضافة صور لعدة أنواع
UPDATE vehicle_types SET image_url = 'https://example.com/images/sedan.jpg' WHERE type_name = 'سيدان';
UPDATE vehicle_types SET image_url = 'https://example.com/images/suv.jpg' WHERE type_name = 'SUV';
UPDATE vehicle_types SET image_url = 'https://example.com/images/van.jpg' WHERE type_name = 'فان';
\`\`\`

## مصادر الصور

يمكنك استخدام:

1. **خدمات التخزين السحابي**:
   - AWS S3
   - Google Cloud Storage
   - Cloudinary
   - ImgBB

2. **روابط خارجية**:
   - يمكنك استخدام روابط صور من الإنترنت (تأكد من حقوق الاستخدام)

3. **رفع الصور محلياً**:
   - يمكنك رفع الصور على السيرفر الخاص بك

## مثال عملي

### إضافة أنواع السيارات الشائعة مع صورها

\`\`\`sql
-- سيدان
INSERT INTO vehicle_types (type_name, type_name_en, image_url, sort_order)
VALUES ('سيدان', 'Sedan', 'https://your-cdn.com/images/sedan.jpg', 1);

-- SUV
INSERT INTO vehicle_types (type_name, type_name_en, image_url, sort_order)
VALUES ('دفع رباعي', 'SUV', 'https://your-cdn.com/images/suv.jpg', 2);

-- فان
INSERT INTO vehicle_types (type_name, type_name_en, image_url, sort_order)
VALUES ('فان', 'Van', 'https://your-cdn.com/images/van.jpg', 3);

-- ليموزين
INSERT INTO vehicle_types (type_name, type_name_en, image_url, sort_order)
VALUES ('ليموزين', 'Limousine', 'https://your-cdn.com/images/limo.jpg', 4);
\`\`\`

## كيف يعمل النظام

1. عندما يتم جلب بيانات السيارات للشريك، يتم عمل JOIN مع جدول `vehicle_types`
2. يتم جلب `image_url` من جدول `vehicle_types`
3. في الـ Frontend، يتم عرض الصورة من `image_url`
4. إذا لم تكن الصورة موجودة أو فشل تحميلها، يتم عرض أيقونة سيارة كـ fallback

## الـ Query المستخدم

\`\`\`sql
SELECT 
  v.vehicle_id,
  v.plate_number,
  v.brand,
  v.model,
  v.manufacture_year,
  v.color,
  v.is_active,
  v.seats,
  vt.type_name,
  vt.type_name_en,
  vt.image_url,  -- ← هنا يتم جلب الصورة
  COUNT(o.operation_id) AS trips_count
FROM public.vehicles v
LEFT JOIN public.vehicle_types vt ON vt.vehicle_type_id = v.vehicle_type_id
LEFT JOIN public.operations o ON o.vehicle_id = v.vehicle_id
WHERE v.partner_id = $1
GROUP BY v.vehicle_id, vt.type_name, vt.type_name_en, vt.image_url
ORDER BY v.created_at DESC
\`\`\`

## ملاحظات مهمة

- **الصور مرتبطة بنوع السيارة** وليس بالسيارة نفسها
- هذا يعني أن جميع السيارات من نفس النوع ستعرض نفس الصورة
- إذا أردت صور مختلفة لكل سيارة، يجب إضافة حقل `image_url` في جدول `vehicles` نفسه

## للمستقبل: صور فردية لكل سيارة

إذا أردت إضافة صور مخصصة لكل سيارة:

\`\`\`sql
-- إضافة حقل جديد في جدول vehicles
ALTER TABLE vehicles ADD COLUMN vehicle_image_url VARCHAR(500);

-- ثم يمكنك تحديث الـ query ليستخدم صورة السيارة إذا كانت موجودة، وإلا يستخدم صورة النوع
SELECT 
  v.vehicle_id,
  COALESCE(v.vehicle_image_url, vt.image_url) as image_url,
  -- ... باقي الحقول
FROM vehicles v
LEFT JOIN vehicle_types vt ON vt.vehicle_type_id = v.vehicle_type_id
\`\`\`
