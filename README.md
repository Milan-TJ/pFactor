
# 🛍️ Product Review System - Django REST API

A simple backend MVP for managing products and their reviews using Django + Django REST Framework.

## 📦 Features

- User registration and token-based authentication
- Admins can create, update, and delete products
- Regular users can post **one review per product**
- All users can view product details and aggregated ratings
- Role-based permission control (Admin vs Regular User)

---

## 🚀 Getting Started

### 1. Clone and Set Up

```bash
git clone <repo_url>
cd pfact
python -m venv env
source env/bin/activate   # or env\Scripts\activate on Windows
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

---

## 🔐 Authentication

We use **token authentication**.

### Register

`POST /api/auth/register/`

```json
{
  "username": "john",
  "password": "password123",
  "is_staff": false
}
```

### Login (Get Token)

`POST /api/auth/login/`

```json
{
  "username": "john",
  "password": "password123"
}
```

**Response:**
```json
{
  "token": "abcd1234...",
  "user_id": 1,
  "username": "john",
  "is_staff": false
}
```

Use the token in headers:
```
Authorization: Token abcd1234...
```

---

## 📘 API Endpoints

### ✅ Product API

#### Create Product (Admin Only)
`POST /api/products/`

```json
{
  "name": "iPhone 15",
  "description": "Latest iPhone model.",
  "price": 999.99
}
```

#### List All Products
`GET /api/products/`

#### Retrieve Product Details
`GET /api/products/<id>/`

**Response:**
```json
{
  "id": 1,
  "name": "iPhone 15",
  "description": "Latest iPhone model.",
  "price": "999.99",
  "average_rating": 4.5,
  "reviews": [
    {
      "id": 3,
      "product": 1,
      "user": 5,
      "rating": 5,
      "feedback": "Amazing!"
    }
  ]
}
```

#### Update Product (Admin Only)
`PUT /api/products/<id>/`

#### Delete Product (Admin Only)
`DELETE /api/products/<id>/`

---

### 📝 Review API

#### Submit a Review (Regular Users)
`POST /api/reviews/`

```json
{
  "product": 1,
  "rating": 4,
  "feedback": "Very good product!"
}
```

> ⚠️ Each user can submit only **one review per product**.

#### List All Reviews
`GET /api/reviews/`

#### Reviews for a Product
Handled via `ProductSerializer.reviews` inside `GET /api/products/<id>/`

---

## ✅ Role-Based Access

| Role         | Can Add Products | Can Review Products | Can View |
|--------------|------------------|---------------------|----------|
| Admin        | ✅                | ❌                  | ✅        |
| Regular User | ❌                | ✅                  | ✅        |
| Anonymous    | ❌                | ❌                  | ✅        |

---

## 🧪 Testing with cURL/Postman

### Register:
```bash
curl -X POST http://localhost:8000/api/auth/register/ \
  -H "Content-Type: application/json" \
  -d '{"username":"john", "password":"pass123", "is_staff": false}'
```

### Login:
```bash
curl -X POST http://localhost:8000/api/auth/login/ \
  -H "Content-Type: application/json" \
  -d '{"username":"john", "password":"pass123"}'
```

---

## 🛠️ Tech Stack

- Python 3.x
- Django 4.x
- Django REST Framework
- Token Authentication

---

## 📄 License

MIT
