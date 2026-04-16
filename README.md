# ☕ بن الكويبي - نظام الإدارة الكامل
## دليل الإعداد خطوة بخطوة

---

## 📁 ملفات النظام

| الملف | الوظيفة |
|-------|---------|
| `index.html` | المتجر الإلكتروني للعملاء |
| `login.html` | صفحة تسجيل الدخول |
| `admin.html` | لوحة التحكم الكاملة |
| `cashier.html` | نظام الكاشير وطباعة الفواتير |
| `Code.gs` | كود Google Apps Script (الخادم) |

---

## 🚀 خطوات الإعداد

### الخطوة 1: إنشاء Google Sheet

1. اذهب إلى [sheets.google.com](https://sheets.google.com)
2. أنشئ جدول بيانات جديد
3. سمّه "بن الكويبي - قاعدة البيانات"
4. **انسخ ID الجدول من الرابط:**
   - الرابط يكون: `https://docs.google.com/spreadsheets/d/[ID_HERE]/edit`
   - انسخ الجزء بين `/d/` و `/edit`

---

### الخطوة 2: إعداد Google Apps Script

1. في Google Sheet، اضغط **Extensions** → **Apps Script**
2. احذف الكود الافتراضي
3. الصق كود `Code.gs` بالكامل
4. في السطر الأول، ضع ID الجدول:
   ```javascript
   const SHEET_ID = 'YOUR_GOOGLE_SHEET_ID_HERE';
   ```
5. اضغط **Save** (Ctrl+S)
6. من القائمة، اختر `setupDatabase` ثم اضغط **Run**
7. وافق على الصلاحيات المطلوبة

**نشر السكريبت:**
1. اضغط **Deploy** → **New Deployment**
2. اختر النوع: **Web app**
3. اضبط:
   - Execute as: **Me**
   - Who has access: **Anyone**
4. اضغط **Deploy**
5. **انسخ الرابط الظاهر** (سيبدأ بـ `https://script.google.com/macros/s/...`)

---

### الخطوة 3: رفع الملفات على GitHub Pages

1. اذهب إلى [github.com](https://github.com) وسجل دخولك
2. أنشئ repository جديد باسم: `bunkuwaybe`
3. ارفع الملفات الأربعة: `index.html`, `login.html`, `admin.html`, `cashier.html`
4. اذهب إلى **Settings** → **Pages**
5. اختر Source: **Deploy from a branch** → `main` → `/ (root)`
6. ستحصل على رابط مثل: `https://username.github.io/bunkuwaybe/`

---

### الخطوة 4: ربط النظام بالـ API

**الطريقة الأولى (موصى بها) - من إعدادات النظام:**
1. سجل دخولك كمدير: `admin` / `admin123`
2. اذهب إلى **الإعدادات** في لوحة التحكم
3. في حقل "رابط Google Apps Script API"، الصق الرابط
4. اضغط **حفظ الإعدادات**

**الطريقة الثانية - تعديل الكود:**
استبدل `YOUR_GOOGLE_APPS_SCRIPT_URL` في كل ملف HTML برابط الـ API.

---

### الخطوة 5: إعداد الواتساب

1. سجل دخول كمدير → الإعدادات
2. أدخل رقم الواتساب بصيغة دولية بدون + مثل: `201012345678`
3. عدّل نص الرسالة حسب رغبتك
4. احفظ الإعدادات

---

## 👥 بيانات الدخول الافتراضية

| المستخدم | كلمة المرور | الصلاحيات |
|---------|------------|----------|
| `admin` | `admin123` | كل الصلاحيات + الإعدادات |
| `cashier` | `cashier123` | الكاشير + فواتير |
| `staff` | `staff123` | عرض فقط |

**⚠️ مهم:** غيّر كلمات المرور الافتراضية من لوحة التحكم → المستخدمون

---

## 🖨️ الطباعة الحرارية

نظام الكاشير مُهيأ للطباعة على:
- طابعة حرارية 80mm (76mm عرض الورقة)
- عند الضغط على "إتمام البيع" يفتح نافذة الفاتورة تلقائياً
- اضغط "طباعة" أو `Ctrl+P`

**إعداد الطابعة في المتصفح:**
- اضبط الهامش على: None
- حجم الورق: 80mm × Auto
- اختر طابعتك الحرارية

---

## 📧 إعداد الإيميلات التلقائية (اختياري)

لتفعيل إرسال تقارير يومية:
1. في Google Apps Script
2. اضغط **Triggers** (ساعة الرنين)
3. أضف Trigger جديد:
   - Function: `sendDailyReport`
   - Event: **Time-driven** → **Day timer** → 9:00 AM

أضف هذه الدالة في Code.gs:
```javascript
function sendDailyReport() {
  const dashboard = getDashboard().data;
  const email = getSettings().data['إيميل_المحل'];
  if (!email) return;
  
  GmailApp.sendEmail(email, 
    '📊 تقرير يومي - بن الكويبي',
    `مبيعات اليوم: ${dashboard.todaySales} جنيه\n` +
    `طلبات معلقة: ${dashboard.pendingOrders}\n` +
    `إجمالي العملاء: ${dashboard.totalCustomers}\n` +
    `منتجات تحت الحد: ${dashboard.lowStock}`
  );
}
```

---

## 🔒 الأمان

- كلمات المرور مخزنة في Google Sheets
- للأمان الأعلى، استخدم Google Apps Script مع التحقق من الطلبات
- يُنصح بتغيير كلمات المرور الافتراضية فور الإعداد

---

## 🆘 حل المشكلات الشائعة

**المشكلة:** لا تظهر المنتجات
**الحل:** تأكد من رابط API في الإعدادات وأن السكريبت منشور بصلاحية "Anyone"

**المشكلة:** رسالة الواتساب لا تفتح
**الحل:** تأكد من رقم الواتساب بصيغة دولية بدون علامة +

**المشكلة:** تسجيل الدخول لا يعمل مع API
**الحل:** في المرحلة التجريبية يعمل بالبيانات الافتراضية (admin/admin123) حتى تربط الـ API

---

## 📞 ملخص الروابط بعد الإعداد

- **المتجر:** `https://username.github.io/bunkuwaybe/`
- **الدخول:** `https://username.github.io/bunkuwaybe/login.html`
- **الإدارة:** `https://username.github.io/bunkuwaybe/admin.html`
- **الكاشير:** `https://username.github.io/bunkuwaybe/cashier.html`

---
*بن الكويبي - نظام مجاني 100% | GitHub Pages + Google Apps Script + Google Sheets*
