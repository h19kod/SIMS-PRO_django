# 🛠️ دليل التثبيت الكامل — SIMS PRO

نظام إدارة معلومات الطلاب (Student Information Management System) مبني على Django + SQLite + Bootstrap 5.

---

## 📋 المتطلبات الأساسية (Prerequisites)

| المتطلب | الإصدار المطلوب |
|---------|----------------|
| Python  | 3.10 أو أعلى   |
| pip     | مرفق مع Python  |
| Git     | أي إصدار حديث  |

للتحقق من التثبيت:

```bash
python --version
pip --version
git --version
```

---

## 📥 الخطوة 1 — استنساخ المشروع (Clone)

```bash
git clone https://github.com/your-username/SIMS.git
cd SIMS/sims_project/sims_project
```

> إذا حصلت على المشروع كـ ZIP، قم بفك الضغط ثم انتقل لمجلد `sims_project/sims_project`.

---

## 🐍 الخطوة 2 — إنشاء البيئة الافتراضية (Virtual Environment)

**Windows (PowerShell):**
```powershell
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

> ✅ ستعرف أن البيئة مفعّلة عندما يظهر `(venv)` في بداية السطر.

---

## 📦 الخطوة 3 — تثبيت المتطلبات (Install Dependencies)

```bash
pip install django
```

للتحقق من تثبيت Django:
```bash
python -m django --version
```

---

## ⚙️ الخطوة 4 — إعداد قاعدة البيانات (Database Setup)

تشغيل الـ migrations لإنشاء الجداول في قاعدة بيانات SQLite:

```bash
python manage.py makemigrations
python manage.py migrate
```

سيتم إنشاء ملف `db.sqlite3` تلقائياً داخل مجلد المشروع.

---

## 👤 الخطوة 5 — إنشاء حساب المدير (Superuser)

```bash
python manage.py createsuperuser
```

سيطلب منك:
- **Username** — اسم المستخدم
- **Email** — البريد الإلكتروني (اختياري)
- **Password** — كلمة المرور (تُكتب دون ظهور على الشاشة)

---

## 🚀 الخطوة 6 — تشغيل السيرفر (Run Server)

```bash
python manage.py runserver
```

افتح المتصفح على:

| الرابط | الوصف |
|--------|-------|
| `http://127.0.0.1:8000/` | الصفحة الرئيسية (قائمة الطلاب) |
| `http://127.0.0.1:8000/students/` | إدارة الطلاب |
| `http://127.0.0.1:8000/management/subjects/` | إدارة المواد |
| `http://127.0.0.1:8000/admin/` | لوحة تحكم Django |

---

## 🗂️ هيكلية المشروع (Project Structure)

```
SIMS/
├── docs/
│   ├── system_overview.md
│   ├── INSTALLATION.md          ← هذا الملف
│   └── API_DOCUMENTATION.md
└── sims_project/
    └── sims_project/
        ├── manage.py
        ├── db.sqlite3
        ├── student_system/      ← إعدادات Django الرئيسية
        │   ├── settings.py
        │   ├── urls.py
        │   ├── wsgi.py
        │   └── asgi.py
        ├── students/            ← تطبيق إدارة الطلاب
        │   ├── models.py
        │   ├── views.py
        │   ├── urls.py
        │   └── admin.py
        ├── management/          ← تطبيق المواد والدرجات
        │   ├── models.py
        │   ├── views.py
        │   ├── urls.py
        │   └── admin.py
        └── templates/
            ├── base.html
            ├── students/
            └── management/
```

---

## 🔧 الإعدادات المهمة في `settings.py`

| الإعداد | القيمة الحالية | الوصف |
|---------|---------------|-------|
| `DEBUG` | `True` | وضع التطوير — غيّره لـ `False` في الإنتاج |
| `LANGUAGE_CODE` | `ar-sa` | واجهة عربية |
| `TIME_ZONE` | `UTC` | المنطقة الزمنية |
| `DATABASES` | SQLite | قاعدة البيانات المستخدمة |

---

## ⚠️ ملاحظات مهمة

- **لا ترفع ملف `db.sqlite3`** إلى GitHub لأنه يحتوي على بيانات حقيقية.
- **`SECRET_KEY`** الموجود في `settings.py` مخصص للتطوير فقط — غيّره قبل النشر الإنتاجي.
- تأكد دائماً من أن البيئة الافتراضية `(venv)` مفعّلة قبل تشغيل أي أمر.

---

## 🆘 حل المشكلات الشائعة

**المشكلة: `ModuleNotFoundError: No module named 'django'`**
```bash
# تأكد أن البيئة الافتراضية مفعّلة أولاً
venv\Scripts\activate   # Windows
pip install django
```

**المشكلة: `django.db.utils.OperationalError: no such table`**
```bash
python manage.py makemigrations students management
python manage.py migrate
```

**المشكلة: المنفذ 8000 مشغول**
```bash
python manage.py runserver 8080
# ثم افتح http://127.0.0.1:8080/
```
