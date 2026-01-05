📌 FIRST PROJECT: Mini Marketplace (Python + SQLAlchemy ORM) – Mukammal Versiya
🎯 Loyihaning maqsadi

Bu loyiha Python + SQLAlchemy ORM yordamida yaratiladi. Siz:

ORM bilan ishlashni amaliy o‘rganasiz

Jadvallar va ularning bog‘lanishlarini tushunasiz

Real hayotdagi marketplace logikasini kodga o‘tkazasiz

💡 Junior-friendly: Bu loyiha database → model → service → business logic → API-ready structure ni tushunishga yordam beradi.

👤 Foydalanuvchi rollari
1️⃣ SELLER (Sotuvchi)

O‘z do‘koniga ega

Mahsulot qo‘shadi

Mahsulot maydonlari:

nomi

narxi

kategoriyasi

description

stock (soni)

2️⃣ CUSTOMER (Xaridor)

Mahsulotlarni ko‘radi

Savatchaga mahsulot qo‘shadi

Savatcha:

mahsulot nomlari

har bir mahsulot soni

umumiy summa hisoblanadi

🧠 Ish jarayoni (flow)

User ro‘yxatdan o‘tadi (email + telefon + password)

User roli belgilanadi (SELLER yoki CUSTOMER)

Agar SELLER bo‘lsa:

Do‘kon yaratiladi

Mahsulotlar qo‘shadi

Agar CUSTOMER bo‘lsa:

Avtomatik savatcha yaratiladi

Mahsulotlarni savatchaga qo‘shadi

CUSTOMER savatchani ko‘radi va umumiy narx hisoblanadi

CUSTOMER mahsulotga baho beradi (rating)

💡 Service layer qat’iy qoidalarni tekshiradi.

🗂️ Ma’lumotlar bazasi jadvallari
1️⃣ users
field	izoh
id	primary key
username	unique, optional
email	unique, login uchun
phone	unique, login uchun
password	hashed
role	SELLER / CUSTOMER
is_active	boolean
created_at	datetime

✅ Har bir user email va telefon orqali login qilishi mumkin.

2️⃣ sellers
field	izoh
id	primary key
user_id	FK → users
shop_name	unique

🔗 1 User → 1 Seller

3️⃣ categories
field	izoh
id	primary key
name	unique

🔗 1 Category → ko‘p Product

4️⃣ products
field	izoh
id	primary key
name	mahsulot nomi
price	narx
description	qisqa tavsif
stock	soni
seller_id	FK → sellers
category_id	FK → categories
created_at	datetime

🔗 1 Seller → ko‘p Product
🔗 1 Category → ko‘p Product

5️⃣ carts
field	izoh
id	primary key
user_id	FK → users

🔗 1 User → 1 Cart

6️⃣ cart_items
field	izoh
id	primary key
cart_id	FK → carts
product_id	FK → products
quantity	nechta

🔗 1 Cart → ko‘p CartItem
🔗 1 Product → ko‘p CartItem

7️⃣ ratings
field	izoh
id	primary key
user_id	FK → users (CUSTOMER)
product_id	FK → products
score	1–5
comment	ixtiyoriy
created_at	datetime

🔒 Qoidalar:

Faqat CUSTOMER baho qo‘ya oladi

Bitta user bitta productga faqat 1 marta rating beradi

🔐 Qat’iy qoidalar (Service layer’da tekshirish)

❌ CUSTOMER mahsulot yaratolmaydi

❌ SELLER savatchaga mahsulot qo‘sha olmaydi

❌ SELLER rating qo‘ya olmaydi

❌ Bitta productga 1 user 2 marta rating bera olmaydi

❌ Stock yetarli bo‘lmasa savatchaga qo‘shilmaydi

🧪 Step-by-step vazifalar
1️⃣ ORM modellarni yaratish

Yuqoridagi 7 ta jadval uchun SQLAlchemy model

ForeignKey va relationship’larni belgilash

2️⃣ UserService

create_user(email, phone, password, role) → hash password

get_user_by_email(email) / get_user_by_phone(phone)

activate_user(id) / deactivate_user(id)

Role va login uchun email + telefonni tekshirish

3️⃣ ProductService

create_product(seller_user, name, price, category_id, description, stock)

Faqat SELLER qo‘shishi mumkin

update_stock(product_id, quantity)

list_products(category_id=None, seller_id=None)

4️⃣ CartService

add_to_cart(customer_user, product_id, quantity)

Agar product allaqachon bo‘lsa → quantity oshadi

Stock yetarli bo‘lmasa → xato

remove_from_cart(customer_user, product_id)

get_cart_summary(customer_user) → total price

5️⃣ RatingService

rate_product(customer_user, product_id, score, comment)

Faqat CUSTOMER

1 productga 1 user 1 marta

📁 Fayl strukturasi
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
│   ├── user_service.py
│   ├── product_service.py
│   ├── cart_service.py
│   └── rating_service.py
│
├── main.py
└── README.md

🚀 Nima o‘rganasiz?

SQLAlchemy ORM asoslari

One-to-Many & One-to-One relationship

Real biznes qoidalarini kodga o‘tkazish

Toza loyiha strukturasini yaratish

Junior → Mid-level backend transition uchun tayyor base

🌟 Future roadmap

Order qo‘shish (checkout)

Rating o‘rtacha bahosini hisoblash

Product filter va search

JWT auth + login token