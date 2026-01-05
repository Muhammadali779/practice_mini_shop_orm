# 📌 FIRST PROJECT: Mini Marketplace (Python ORM)

## 🎯 Loyihaning maqsadi

Bu loyiha **Python + SQLAlchemy ORM** yordamida quriladigan
**oddiy marketplace ma’lumotlar bazasi**.

Loyihaning asosiy maqsadi:

* ORM bilan ishlashni o‘rganish
* jadvallar o‘rtasidagi bog‘lanishlarni tushunish
* real hayotdagi marketplace logikasini tushunish

---

## 👤 Foydalanuvchi rollari

Tizimda **ikki xil foydalanuvchi** mavjud:

### 1. SELLER (Sotuvchi)

* O‘z do‘koniga ega
* Mahsulot qo‘sha oladi
* Mahsulotga quyidagilarni kiritadi:

  * nomi
  * narxi
  * kategoriyasi
  * description
  * stock (soni)

### 2. CUSTOMER (Xaridor)

* Mahsulotlarni ko‘radi
* Savatchaga mahsulot qo‘shadi
* Savatchada:

  * mahsulot nomlari
  * har bir mahsulot soni
  * umumiy summa chiqadi

---

## 🧠 Asosiy ish jarayoni (Qanday ishlaydi?)

1. User ro‘yxatdan o‘tadi
2. User roli belgilanadi (`SELLER` yoki `CUSTOMER`)
3. Agar user SELLER bo‘lsa:

   * do‘kon yaratiladi
   * mahsulotlar qo‘sha oladi
4. Agar user CUSTOMER bo‘lsa:

   * avtomatik savatcha yaratiladi
   * mahsulotlarni savatchaga qo‘shadi
5. CUSTOMER savatchani ko‘radi va umumiy narx hisoblanadi

---

## 🗂️ Ma’lumotlar bazasi jadvallari

### 1️⃣ users

Barcha foydalanuvchilar

| field      | izoh              |
| ---------- | ----------------- |
| id         | primary key       |
| username   | unique            |
| email      | unique            |
| password   | hashed            |
| role       | SELLER / CUSTOMER |
| created_at | datetime          |

---

### 2️⃣ sellers

Faqat sotuvchilar uchun

| field     | izoh        |
| --------- | ----------- |
| id        | primary key |
| user_id   | FK → users  |
| shop_name | do‘kon nomi |

🔗 Aloqa: **1 User → 1 Seller**

---

### 3️⃣ categories

Mahsulot kategoriyalari

| field | izoh        |
| ----- | ----------- |
| id    | primary key |
| name  | unique      |

🔗 Aloqa: **1 Category → ko‘p Product**

---

### 4️⃣ products

Sotiladigan mahsulotlar

| field       | izoh            |
| ----------- | --------------- |
| id          | primary key     |
| name        | mahsulot nomi   |
| price       | narx            |
| description | qisqa tavsif    |
| stock       | soni            |
| seller_id   | FK → sellers    |
| category_id | FK → categories |
| created_at  | datetime        |

---

### 5️⃣ carts

Xaridor savatchasi

| field   | izoh        |
| ------- | ----------- |
| id      | primary key |
| user_id | FK → users  |

🔗 Aloqa: **1 User → 1 Cart**

---

### 6️⃣ cart_items

Savatchadagi mahsulotlar

| field      | izoh          |
| ---------- | ------------- |
| id         | primary key   |
| cart_id    | FK → carts    |
| product_id | FK → products |
| quantity   | nechta        |

---

### 7️⃣ ratings

Mahsulot baholari (rating)

| field      | izoh                  |
| ---------- | --------------------- |
| id         | primary key           |
| user_id    | FK → users (CUSTOMER) |
| product_id | FK → products         |
| score      | 1–5                   |
| comment    | ixtiyoriy             |
| created_at | datetime              |

🔒 **Qoidalar**:

* Faqat CUSTOMER baho qo‘ya oladi
* Bitta user bitta productga faqat **1 marta** rating beradi

---

## 🔐 Qat’iy qoidalar (Juda muhim)

Bularni **service layer’da tekshirishing shart**:

* ❌ CUSTOMER mahsulot yarata olmaydi
* ❌ SELLER savatchaga mahsulot qo‘sha olmaydi
* ❌ SELLER rating qo‘ya olmaydi
* ❌ Bitta productga 1 user 2 marta rating bera olmaydi
* ❌ Stock yetarli bo‘lmasa savatchaga qo‘shilmaydi

---

## 🧪 ANIQLASHTIRILGAN VAZIFALAR (STEP-BY-STEP)

### ✅ 1-qadam: ORM modellarni yoz

* Yuqoridagi **7 ta jadval** uchun model yoz
* `ForeignKey` va `relationship` ishlat

---

### ✅ 2-qadam: User logikasini yoz

* User yaratish
* User rolini tekshirish

---

### ✅ 3-qadam: Product qo‘shish

```python
create_product(seller_user, name, price, category_id, description, stock)
```

✔️ Faqat SELLER ishlata oladi

---

### ✅ 4-qadam: Savatchaga qo‘shish

```python
add_to_cart(customer_user, product_id, quantity)
```

✔️ Agar product allaqachon bo‘lsa → quantity oshadi
❌ Agar stock yetarli bo‘lmasa → xato

---

### ✅ 5-qadam: Savatchani ko‘rish

```python
get_cart_summary(customer_user)
```

Natija:

```text
Laptop x2 = 2000
Mouse  x1 = 50
----------------
Total: 2050
```

---

### ✅ 6-qadam: Rating qo‘yish

```python
rate_product(customer_user, product_id, score, comment)
```

---

## 📁 Project fayl strukturasi

```
marketplace/
├── database/
│   ├── base.py
│   └── session.py
│
├── models/
│   ├── user.py
│   ├── seller.py
│   ├── category.py
│   ├── product.py
│   ├── cart.py
│   ├── cart_item.py
│   └── rating.py
│
├── services/
│   ├── product_service.py
│   ├── cart_service.py
│   └── rating_service.py
│
├── main.py
└── README.md
```

---

## 🧭 Bu loyiha orqali nimani o‘rganasan?

* SQLAlchemy ORM asoslari
* One-to-Many relationship
* Real biznes qoidalarini kodga o‘tkazish
* Toza loyiha strukturasi

---

## 🚀 Keyingi qadamlar (keyinroq)

* Order qo‘shish
* Rating o‘rtacha bahosini hisoblash
* Product filter va search

---
