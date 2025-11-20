### 1️⃣ واجهة المستخدم (User API)
**المسؤول:** `Content Delivery API`
**الغرض:** قراءة البيانات فقط (Read Only).

#### **أ) جلب قائمة الإدعاءات (List Claims)**
*   **الرابط:** `GET /claims?limit=20&page=1`
*   **جسم الطلب (Request Body):** *لا يوجد (فارغ).*
*   **جسم الرد (Response Body):**
    ```json
    [
      {
        "claim_id": 21312,
        "title": "عنوان الإدعاء الأول",
        "summary": "ملخص قصير للإدعاء يظهر في القائمة...",
        "image_url": "https://cdn.myapp.com/images/img1.jpg"
      },
      {
        "claim_id": 21313,
        "title": "عنوان الإدعاء الثاني",
        "summary": "ملخص قصير...",
        "image_url": "https://cdn.myapp.com/images/img2.jpg"
      }
      // ... يتم تكرارها حتى 20 عنصرًا
    ]
    ```

#### **ب) جلب تفاصيل إدعاء واحد (Get Single Claim)**
*   **الرابط:** `GET /claims/21312`
*   **جسم الطلب (Request Body):** *لا يوجد.*
*   **جسم الرد (Response Body):**
    ```json
    {
      "claim_id": 21312,
      "title": "العنوان الكامل للإدعاء",
      "summary": "الملخص...",
      "image_url": "https://cdn.myapp.com/images/img_full.jpg",
      "content": [
        {
          "header": "المقدمة",
          "body": "نص الفقرة الأولى بالتفصيل..."
        },
        {
          "header": "الأدلة",
          "body": "نص الفقرة الثانية..."
        },
        {
          "header": "الخاتمة",
          "body": "نص الفقرة الثالثة..."
        }
      ]
    }
    ```

---

### 2️⃣ خدمة المصادقة (Authentication API)
**المسؤول:** `Authentication Service`
**الغرض:** التحقق من الهوية وإصدار الـ JWT.

#### **أ) تسجيل دخول تقليدي (Email Login)**
*   **الرابط:** `POST /login`
*   **جسم الطلب (Request Body):**
    ```json
    {
      "email": "admin@example.com",
      "password": "secret_password_123"
    }
    ```
*   **جسم الرد (Response Body):** *نفس الرد في الحالتين (أ و ب)*.

#### **ب) تسجيل دخول جوجل (Google Login)**
*   **الرابط:** `POST /login/google`
*   **جسم الطلب (Request Body):**
    ```json
    {
      "google_token": "eyJhbGciOiJSUzI1NiIsIm..." // الرمز القادم من جوجل
    }
    ```

#### **ج) الرد الموحد للنجاح (Success Response)**
*   **الحالة:** `200 OK`
    ```json
    {
      "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...", // JWT الخاص بنظامك
      "token_type": "Bearer"
    }
    ```

---

### 3️⃣ خدمة النشر والإدارة (Publishing API)
**المسؤول:** `Publishing Service`
**الغرض:** إنشاء المحتوى ورفع الملفات (محمي بـ Token).

#### **أ) رفع صورة (Upload Image)**
*   **الرابط:** `POST /upload`
*   **الهيدر (Header):** `Authorization: Bearer <TOKEN>`
*   **جسم الطلب (Request Body):** *ملف ثنائي (Binary File) - ليس JSON.*
*   **جسم الرد (Response Body):**
    ```json
    {
      "url": "https://cdn.myapp.com/images/photo_123.jpg"
    }
    ```

#### **ب) إنشاء إدعاء جديد (Create Claim)**
*   **الرابط:** `POST /claims`
*   **الهيدر (Header):** `Authorization: Bearer <TOKEN>`
*   **جسم الطلب (Request Body):** *لاحظ أننا نستخدم رابط الصورة الذي حصلنا عليه من الخطوة السابقة.*
    ```json
    {
      "title": "عنوان الإدعاء الجديد",
      "summary": "ملخص الإدعاء...",
      "image_url": "https://cdn.myapp.com/images/photo_123.jpg",
      "content": [
        {
          "header": "العنوان الفرعي الأول",
          "body": "محتوى الفقرة الأولى..."
        },
        {
          "header": "العنوان الفرعي الثاني",
          "body": "محتوى الفقرة الثانية..."
        }
      ]
    }
    ```
*   **جسم الرد (Response Body):**
    *   **الحالة:** `201 Created`
    ```json
    {
      "claim_id": 54879, // الرقم الجديد الذي تم إنشاؤه
      "title": "عنوان الإدعاء الجديد",
      "summary": "ملخص الإدعاء...",
      "image_url": "https://cdn.myapp.com/images/photo_123.jpg",
      "content": [
         // ... نفس المحتوى المرسل
      ]
    }
    ```
