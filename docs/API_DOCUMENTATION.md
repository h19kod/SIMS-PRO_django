# 📡 توثيق الـ API — SIMS PRO

توثيق كامل لجميع مسارات (Endpoints) نظام SIMS PRO.  
النظام يعتمد على **Django Views** بشكل HTML (ليس REST API)، وكل طلب يُعيد صفحة HTML أو يُعيد توجيهاً (Redirect).

**Base URL:** `http://127.0.0.1:8000`

---

## 📚 فهرس المحتوى

- [🎓 Students — إدارة الطلاب](#-students--إدارة-الطلاب)
- [📖 Management — إدارة المواد والدرجات](#-management--إدارة-المواد-والدرجات)
- [🔢 نماذج البيانات (Data Models)](#-نماذج-البيانات-data-models)
- [🔄 منطق الاستجابة (Response Logic)](#-منطق-الاستجابة-response-logic)

---

## 🎓 Students — إدارة الطلاب

Base Path: `/students/`

---

### `GET /students/`
**الوظيفة:** عرض قائمة جميع الطلاب مع إحصائيات سريعة.

**Query Parameters (اختيارية):**

| المعامل | النوع   | الوصف |
|---------|---------|-------|
| `q`     | string  | كلمة البحث — يبحث في الاسم الأول أو الأخير أو الرقم الجامعي |

**مثال:**
```
GET /students/?q=أحمد
GET /students/?q=2024001
```

**البيانات المُعادة (Context):**

| المتغير | النوع | الوصف |
|---------|-------|-------|
| `students` | QuerySet | قائمة الطلاب المُفلترة |
| `query` | string | كلمة البحث المُدخلة |
| `total_students` | int | إجمالي عدد الطلاب في النظام |
| `total_departments` | int | إجمالي عدد الأقسام |

**Template:** `templates/students/student_list.html`

---

### `GET /students/add/`
**الوظيفة:** عرض نموذج إضافة طالب جديد.

**البيانات المُعادة (Context):**

| المتغير | النوع | الوصف |
|---------|-------|-------|
| `departments` | QuerySet | قائمة جميع الأقسام للاختيار منها |

**Template:** `templates/students/add_student.html`

---

### `POST /students/add/`
**الوظيفة:** حفظ بيانات طالب جديد في قاعدة البيانات.

**Request Body (Form Data):**

| الحقل | النوع | مطلوب | الوصف |
|-------|-------|--------|-------|
| `first_name` | string | ✅ | الاسم الأول |
| `last_name` | string | ✅ | الاسم الأخير |
| `university_id` | string | ✅ | الرقم الجامعي (فريد) |
| `level` | integer | ✅ | المرحلة الدراسية |
| `department` | integer | ✅ | ID القسم |

**الاستجابة عند النجاح:** `302 Redirect → /students/`

---

### `GET /students/edit/<pk>/`
**الوظيفة:** عرض نموذج تعديل بيانات طالب موجود.

**URL Parameters:**

| المعامل | النوع | الوصف |
|---------|-------|-------|
| `pk` | integer | المعرّف الفريد للطالب |

**البيانات المُعادة (Context):**

| المتغير | النوع | الوصف |
|---------|-------|-------|
| `student` | Student | بيانات الطالب الحالية |
| `departments` | QuerySet | قائمة الأقسام |

**Template:** `templates/students/edit_student.html`

**الاستجابة عند عدم الوجود:** `404 Not Found`

---

### `POST /students/edit/<pk>/`
**الوظيفة:** تحديث بيانات طالب موجود.

**URL Parameters:**

| المعامل | النوع | الوصف |
|---------|-------|-------|
| `pk` | integer | المعرّف الفريد للطالب |

**Request Body (Form Data):**

| الحقل | النوع | مطلوب | الوصف |
|-------|-------|--------|-------|
| `first_name` | string | ✅ | الاسم الأول |
| `last_name` | string | ✅ | الاسم الأخير |
| `university_id` | string | ✅ | الرقم الجامعي |
| `level` | integer | ✅ | المرحلة الدراسية |
| `department` | integer | ✅ | ID القسم |

**الاستجابة عند النجاح:** `302 Redirect → /students/`

---

### `GET /students/delete/<pk>/`
**الوظيفة:** حذف طالب نهائياً من قاعدة البيانات.

**URL Parameters:**

| المعامل | النوع | الوصف |
|---------|-------|-------|
| `pk` | integer | المعرّف الفريد للطالب |

**الاستجابة عند النجاح:** `302 Redirect → /students/`  
**الاستجابة عند عدم الوجود:** `404 Not Found`

> ⚠️ **تحذير:** الحذف نهائي ومباشر — لا يمكن التراجع عنه.

---

## 📖 Management — إدارة المواد والدرجات

Base Path: `/management/`

---

### `GET /management/subjects/`
**الوظيفة:** عرض قائمة جميع المواد الدراسية مرتبة حسب القسم ثم الاسم.

**البيانات المُعادة (Context):**

| المتغير | النوع | الوصف |
|---------|-------|-------|
| `subjects` | QuerySet | قائمة المواد مرتبة بـ `department__name, name` |

**Template:** `templates/management/subject_list.html`

---

### `GET /management/subjects/add/`
**الوظيفة:** عرض نموذج إضافة مادة دراسية جديدة.

**البيانات المُعادة (Context):**

| المتغير | النوع | الوصف |
|---------|-------|-------|
| `departments` | QuerySet | قائمة الأقسام للاختيار |

**Template:** `templates/management/add_subject.html`

---

### `POST /management/subjects/add/`
**الوظيفة:** حفظ مادة دراسية جديدة.

**Request Body (Form Data):**

| الحقل | النوع | مطلوب | الوصف |
|-------|-------|--------|-------|
| `name` | string | ✅ | اسم المادة |
| `code` | string | ✅ | كود المادة (فريد، مثل: `CS101`) |
| `department` | integer | ✅ | ID القسم |

**الاستجابة عند النجاح:** `302 Redirect → /management/subjects/`

> ملاحظة: حقل `level` موجود في `Subject` model لكنه غير مُضمَّن في نموذج الإضافة حالياً.

---

### `GET /management/student/<student_id>/grades/`
**الوظيفة:** عرض جميع درجات طالب محدد مع المعدل وتقييمه.

**URL Parameters:**

| المعامل | النوع | الوصف |
|---------|-------|-------|
| `student_id` | integer | المعرّف الفريد للطالب |

**البيانات المُعادة (Context):**

| المتغير | النوع | الوصف |
|---------|-------|-------|
| `student` | Student | بيانات الطالب |
| `grades` | QuerySet | درجاته مرتبة تنازلياً بالفصل |
| `avg` | float | المعدل التراكمي (مقرّب لرقمين عشريين) |
| `status` | string | التقييم النصي (امتياز / جيد جداً / ...) |
| `color` | string | اسم لون Bootstrap للتقييم |

**جدول التقييمات التلقائية:**

| نطاق الدرجة | التقييم | لون Bootstrap |
|------------|---------|--------------|
| 90 — 100   | امتياز  | `success` (أخضر) |
| 80 — 89    | جيد جداً | `primary` (أزرق) |
| 70 — 79    | جيد     | `info` (سماوي) |
| 50 — 69    | مقبول   | `warning` (أصفر) |
| أقل من 50  | راسب    | `danger` (أحمر) |

**Template:** `templates/management/student_grades.html`  
**الاستجابة عند عدم الوجود:** `404 Not Found`

---

### `GET /management/student/<student_id>/add_grade/`
**الوظيفة:** عرض نموذج رصد درجة جديدة لطالب محدد.

**URL Parameters:**

| المعامل | النوع | الوصف |
|---------|-------|-------|
| `student_id` | integer | المعرّف الفريد للطالب |

**البيانات المُعادة (Context):**

| المتغير | النوع | الوصف |
|---------|-------|-------|
| `student` | Student | بيانات الطالب |
| `subjects` | QuerySet | المواد المتاحة لقسم الطالب فقط |

**Template:** `templates/management/add_grade.html`

---

### `POST /management/student/<student_id>/add_grade/`
**الوظيفة:** حفظ درجة جديدة لطالب.

**URL Parameters:**

| المعامل | النوع | الوصف |
|---------|-------|-------|
| `student_id` | integer | المعرّف الفريد للطالب |

**Request Body (Form Data):**

| الحقل | النوع | مطلوب | الوصف |
|-------|-------|--------|-------|
| `subject` | integer | ✅ | ID المادة |
| `score` | decimal | ✅ | الدرجة (حتى 5 أرقام، رقمين عشريين) |
| `semester` | string | ✅ | الفصل الدراسي (مثل: `2025-1`) |

**الاستجابة عند النجاح:** `302 Redirect → /management/student/<student_id>/grades/`

---

### `GET /management/student/delete/<pk>/`
**الوظيفة:** حذف طالب عبر تطبيق management (مسار بديل).

**URL Parameters:**

| المعامل | النوع | الوصف |
|---------|-------|-------|
| `pk` | integer | المعرّف الفريد للطالب |

**الاستجابة عند النجاح:** `302 Redirect → /students/`  
**الاستجابة عند عدم الوجود:** `404 Not Found`

---

## 🔢 نماذج البيانات (Data Models)

### Department (القسم)

| الحقل | النوع | القيود | الوصف |
|-------|-------|--------|-------|
| `id` | AutoField | PK | المعرّف التلقائي |
| `name` | CharField(100) | — | اسم القسم |
| `code` | CharField(10) | unique | الكود المختصر (مثل: `CS`) |

---

### Student (الطالب)

| الحقل | النوع | القيود | الوصف |
|-------|-------|--------|-------|
| `id` | AutoField | PK | المعرّف التلقائي |
| `first_name` | CharField(50) | — | الاسم الأول |
| `last_name` | CharField(50) | — | الاسم الأخير |
| `university_id` | CharField(20) | unique | الرقم الجامعي |
| `email` | EmailField | unique | البريد الإلكتروني |
| `department` | ForeignKey → Department | CASCADE | القسم |
| `level` | IntegerField | default=1 | المرحلة الدراسية |

---

### Subject (المادة)

| الحقل | النوع | القيود | الوصف |
|-------|-------|--------|-------|
| `id` | AutoField | PK | المعرّف التلقائي |
| `name` | CharField(100) | — | اسم المادة |
| `code` | CharField(10) | unique | الكود (مثل: `CS101`) |
| `department` | ForeignKey → Department | CASCADE | القسم |
| `level` | IntegerField | — | المرحلة الدراسية |

---

### Grade (الدرجة)

| الحقل | النوع | القيود | الوصف |
|-------|-------|--------|-------|
| `id` | AutoField | PK | المعرّف التلقائي |
| `student` | ForeignKey → Student | CASCADE | الطالب |
| `subject` | ForeignKey → Subject | CASCADE | المادة |
| `score` | DecimalField(5,2) | — | الدرجة (0.00 — 999.99) |
| `semester` | CharField(20) | — | الفصل الدراسي |

---

## 🔄 منطق الاستجابة (Response Logic)

```
طلب GET  →  عرض صفحة HTML
طلب POST →  معالجة البيانات → Redirect (302) عند النجاح
مسار غير موجود (pk خاطئ) →  404 Not Found (get_object_or_404)
```

**خريطة Redirects بعد العمليات:**

| العملية | Redirect إلى |
|---------|-------------|
| إضافة طالب | `/students/` |
| تعديل طالب | `/students/` |
| حذف طالب | `/students/` |
| إضافة مادة | `/management/subjects/` |
| رصد درجة | `/management/student/<id>/grades/` |

---

## 🔐 الأمان والمصادقة

- النظام حالياً **بدون مصادقة** (Authentication) على الـ Views العامة.
- لوحة الإدارة `/admin/` تتطلب حساب Superuser.
- **CSRF Protection** مفعّلة تلقائياً على جميع طلبات POST عبر Django middleware.
- يجب إضافة `{% csrf_token %}` في كل نموذج HTML.

---

*آخر تحديث: يونيو 2026 — SIMS PRO v1.0*
