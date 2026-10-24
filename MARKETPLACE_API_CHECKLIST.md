# 📋 Rivo Marketplace & Vendor Portal: End-to-End API Testing Checklist

> **ব্যবহার নির্দেশিকা:** ব্রুনোতে প্রতিটি এপিআই টেস্ট করার সময় এই চেকলিস্টটি ব্যবহার করুন। প্রতিটি এপিআই টেস্ট সফল হলে চেকবক্স টিক `[x]` করতে পারেন। কোনো ফিল্ড বা লজিক রিফ্যাক্টরিং লাগলে সাথে সাথে উল্লেখ করবেন।

---

## 🔑 গ্লোবাল ব্রুনো ভ্যারিয়েবলস ও হেডারস

| হেডার / ভ্যারিয়েবল | মান (Value) | বিবরণ |
| :--- | :--- | :--- |
| `base_url` | `http://localhost:81/api` | ব্যাকএন্ড এপিআই বেস ইউআরএল |
| `Accept` | `application/json` | JSON রেসপন্স নিশ্চিত করার জন্য |
| `Authorization` | `Bearer {{userToken}}` | কাস্টমার বা ভেন্ডরের Sanctum টোকেন |

---

## 🛍️ PART 1: Customer Marketplace Module (কাস্টমার ফ্লো)

### 📍 ধাপ ১: ডিসকভারি, ক্যাটালগ ও সার্চ (Discovery & Search)
- [x] **01. GET** `/api/v1/marketplace/home`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Get Marketplace Home.yml`
  - **কাজ:** হোম ফিড লোড (ব্যানার, ক্যাটাগরি, টপ ডিল, রেকমেন্ডেড ও স্টোর)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **02. GET** `/api/v1/marketplace/categories`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Get Categories List.yml`
  - **কাজ:** সব সক্রিয় ক্যাটাগরি, আইকন এবং প্রতিটিতে কয়টি প্রোডাক্ট আছে তার কাউন্ট
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **03. GET** `/api/v1/marketplace/categories/:category/products`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Get Products by Category.yml`
  - **কাজ:** স্পেসিফিক ক্যাটাগরি (যেমন: `food` বা `treats`)-র সব প্রোডাক্ট দেখা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **04. GET** `/api/v1/marketplace/stores`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Browse Nearby Stores.yml`
  - **কাজ:** আশেপাশের সব লাইভ পেট শপ ও ভেন্ডর স্টোর ব্রাউজ করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **05. GET** `/api/v1/marketplace/stores/:store`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Get Store Profile & Products.yml`
  - **কাজ:** নির্দিষ্ট স্টোর প্রোফাইল, ডেলিভারি ফি, রেটিং এবং স্টোরের প্রোডাক্ট তালিকা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **06. GET** `/api/v1/marketplace/products`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Browse Products Catalog.yml`
  - **কাজ:** অল প্রোডাক্টস ক্যাটালগ ব্রাউজ (ফিল্টার: `pet_type`, `price`, `sort`)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **07. GET** `/api/v1/marketplace/products/:product`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Get Product Details.yml`
  - **কাজ:** প্রোডাক্ট পেজ (ছবি, ডেসক্রিপশন, ভ্যারিয়েন্ট, ইনভেন্টরি স্টক ও রিভিউ সামারি)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **08. GET** `/api/v1/marketplace/search`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Unified Search.yml`
  - **কাজ:** ইউনিফাইড সার্চ বার (একসাথে Products এবং Stores ট্যাব)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **09. DELETE** `/api/v1/marketplace/search/recent`
  - **ব্রুনো ফাইল:** `Marketplace/Catalog/Clear Recent Searches.yml`
  - **কাজ:** কাস্টমারের রিসেন্ট সার্চ কি-ওয়ার্ড হিস্ট্রি এক ক্লিকে মুছে ফেলা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

### 📍 ধাপ ২: ডেলিভারি অ্যাড্রেস বুক (Delivery Address Management)
- [x] **10. GET** `/api/v1/marketplace/addresses`
  - **ব্রুনো ফাইল:** `Marketplace/Addresses/Get Saved Addresses.yml`
  - **কাজ:** কাস্টমারের সেভ করা সব ডেলিভারি অ্যাড্রেসের তালিকা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **11. POST** `/api/v1/marketplace/addresses`
  - **ব্রুনো ফাইল:** `Marketplace/Addresses/Save Delivery Address.yml`
  - **কাজ:** নতুন ডেলিভারি অ্যাড্রেস সেভ করা (House no, Street, Landmark, Lat/Lng)
  - **প্রত্যাশিত স্ট্যাটাস:** `201 Created`
- [x] **12. PUT** `/api/v1/marketplace/addresses/:address`
  - **ব্রুনো ফাইল:** `Marketplace/Addresses/Update Delivery Address.yml`
  - **কাজ:** আগের সেভ করা অ্যাড্রেস এডিট / আপডেট করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **13. PATCH** `/api/v1/marketplace/addresses/:address/default`
  - **ব্রুনো ফাইল:** `Marketplace/Addresses/Set Default Address.yml`
  - **কাজ:** যেকোনো একটি অ্যাড্রেসকে প্রাইমারি/ডিফল্ট অ্যাড্রেস হিসেবে সেট করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **14. DELETE** `/api/v1/marketplace/addresses/:address`
  - **ব্রুনো ফাইল:** `Marketplace/Addresses/Delete Address.yml`
  - **কাজ:** অ্যাড্রেস মুছে ফেলা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

### 📍 ধাপ ৩: শপিং কার্ট (Shopping Cart)
- [x] **15. GET** `/api/v1/marketplace/cart`
  - **ব্রুনো ফাইল:** `Marketplace/Cart/Get Cart.yml`
  - **কাজ:** সক্রিয় কার্ট দেখা (আইটেমস, সাবটোটাল, ট্যাক্স ও Rivo Pro $0 ডেলিভারি ডিসকাউন্ট)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **16. POST** `/api/v1/marketplace/cart/items`
  - **ব্রুনো ফাইল:** `Marketplace/Cart/Add Item to Cart.yml`
  - **কাজ:** কার্টে প্রোডাক্ট বা নির্দিষ্ট ভ্যারিয়েন্ট যোগ করা (অটো সিঙ্গেল-স্টোর এনফোর্সমেন্ট)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **17. PATCH** `/api/v1/marketplace/cart/items/:id`
  - **ব্রুনো ফাইল:** `Marketplace/Cart/Update Cart Item Quantity.yml`
  - **কাজ:** কার্টের কোনো আইটেমের পরিমাণ বাড়ানো বা কমানো (+ / -)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **18. DELETE** `/api/v1/marketplace/cart/items/:id`
  - **ব্রুনো ফাইল:** `Marketplace/Cart/Remove Item from Cart.yml`
  - **কাজ:** কার্ট থেকে নির্দিষ্ট একটি আইটেম সরিয়ে ফেলা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **19. DELETE** `/api/v1/marketplace/cart`
  - **ব্রুনো ফাইল:** `Marketplace/Cart/Clear Entire Cart.yml`
  - **কাজ:** পুরো কার্ট এক ক্লিকে খালি করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

### 📍 ধাপ ৪: চেকআউট ও পেমেন্ট ইঞ্জিন (Checkout & Payments)
- [x] **20. POST** `/api/v1/marketplace/checkout`
  - **ব্রুনো ফাইল:** `Marketplace/Orders/Process Checkout.yml`
  - **কাজ:** অর্ডার প্লেস করা এবং ডিকাপলড গেটওয়ে (Stripe PaymentIntent বা COD) ইনিশিয়েট
  - **পেমেন্ট অপশনস:** `stripe`, `apple_pay`, `mastercard`, `visa`, `cash_on_delivery`
  - **প্রত্যাশিত স্ট্যাটাস:** `201 Created`
- [x] **21. POST** `/api/v1/marketplace/orders/:order/confirm-payment`
  - **ব্রুনো ফাইল:** `Marketplace/Orders/Confirm Order Payment.yml`
  - **কাজ:** ক্লায়েন্ট সাইড 3D Secure / Stripe Sheet পেমেন্ট যাচাই করে অর্ডারকে `paid` মার্ক করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **22. GET** `/api/v1/marketplace/orders`
  - **ব্রুনো ফাইল:** `Marketplace/Orders/Get My Orders (Tabs).yml`
  - **কাজ:** কাস্টমারের অর্ডার তালিকা (ট্যাব ফিল্টার: `ongoing`, `completed`, `cancelled`)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **23. GET** `/api/v1/marketplace/orders/:order`
  - **ব্রুনো ফাইল:** `Marketplace/Orders/Get Order Details & Tracking.yml`
  - **কাজ:** অর্ডারের রসিদ, লাইভ স্ট্যাটাস ট্র্যাকার ও আইটেম স্ন্যাপশট দেখা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **24. POST** `/api/v1/marketplace/orders/:order/cancel`
  - **ব্রুনো ফাইল:** `Marketplace/Orders/Cancel Order.yml`
  - **কাজ:** শিপমেন্টের আগে অর্ডার বাতিল করা (ইনভেন্টরি স্টক অটো-রিস্টোর ও গেটওয়ে রিফান্ড)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

### 📍 ধাপ ৫: কাস্টমার রিভিউ ও রেটিং (Reviews & Ratings)
- [x] **25. POST** `/api/v1/marketplace/reviews`
  - **ব্রুনো ফাইল:** `Marketplace/Reviews/Submit Product or Store Review.yml`
  - **কাজ:** কেনা প্রোডাক্ট বা স্টোরের ওপর রেটিং (১-৫ স্টার) ও রিভিউ পোস্ট করা
  - **প্রত্যাশিত স্ট্যাটাস:** `201 Created`
- [x] **26. GET** `/api/v1/marketplace/products/:product/reviews`
  - **ব্রুনো ফাইল:** `Marketplace/Reviews/Get Product Reviews & Breakdown.yml`
  - **কাজ:** প্রোডাক্টের সব রিভিউ এবং স্টার ব্রেকডাউন (5★, 4★, 3★ ...) দেখা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **27. GET** `/api/v1/marketplace/stores/:store/reviews`
  - **ব্রুনো ফাইল:** `Marketplace/Reviews/Get Store Reviews & Breakdown.yml`
  - **কাজ:** স্টোরের সব রিভিউ ও রেটিং পরিসংখ্যান দেখা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`


- [x] **27.1 POST/PUT** `/api/v1/marketplace/reviews/:review` *(Bonus)*
  - **ব্রুনো ফাইল:** `Marketplace/Reviews/Update Review.yml`
  - **কাজ:** কাস্টমারের রিভিউ এডিট করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **27.2 DELETE** `/api/v1/marketplace/reviews/:review` *(Bonus)*
  - **ব্রুনো ফাইল:** `Marketplace/Reviews/Delete Review.yml`
  - **কাজ:** কাস্টমার রিভিউ পার্মানেন্ট ডিলিট করা (ছবি ডিস্ক থেকে মুছে ফেলা, রেটিং রিক্যালকুলেশন ও ক্যাশ ফ্লাশ)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

## 🏪 PART 2: Vendor Portal Module (ভেন্ডর ফ্লো)

### 📍 ধাপ ৬: স্টোর অনবোর্ডিং, প্রোফাইল ও KYC (Store Profile & KYC)
- [x] **28. POST** `/api/v1/vendor/store`
  - **ব্রুনো ফাইল:** `Vendor Portal/Store Profile & KYC/Onboard Store Profile.yml`
  - **কাজ:** ভেন্ডর স্টোর রেজিস্ট্রেশন ও অনবোর্ডিং উইজার্ড (Store name, type, phone, address, delivery fee, logo, banner, KYC docs)। নিরাপদ আপসার্ট সমর্থন করে (নতুন স্টোরের ক্ষেত্রে 201 Created, আর অনবোর্ডিং পুনরায় সাবমিট করলে রেটিং/স্লাগ না মুছে 200 OK)।
  - **রেসপন্স রিসোর্স:** `VendorStoreProfileResource`
  - **প্রত্যাশিত স্ট্যাটাস:** `201 Created` / `200 OK`
- [x] **29. GET** `/api/v1/vendor/profile`
  - **ব্রুনো ফাইল:** `Vendor Portal/Store Profile & KYC/Get Store Profile.yml`
  - **কাজ:** স্টোরের বিজনেস প্রোফাইল, অ্যাসেট ইউআরএল (WebP logo, banner), ভেরিফিকেশন ব্যাজ ও KYC ডকুমেন্ট হিস্ট্রি দেখা।
  - **রেসপন্স রিসোর্স:** `VendorStoreProfileResource`
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **30. POST** `/api/v1/vendor/profile`
  - **ব্রুনো ফাইল:** `Vendor Portal/Store Profile & KYC/Update Store Profile.yml`
  - **কাজ:** স্টোরের প্রোফাইল সেটিংস আপডেট (নাম, টাইপ, ফোন, লোকেশন, ডেলিভারি ফি, ডেলিভারি টাইম, লাইভ স্ট্যাটাস `is_live`, লোগো ও ব্যানার)। পুরনো ছবি ডিস্ক থেকে ডিলিট করে নতুন WebP ছবি সেট করে এবং Redis ক্যাশ ট্যাগ ফ্ল্যাশ করে।
  - **রেসপন্স রিসোর্স:** `VendorStoreProfileResource`
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [x] **31. POST** `/api/v1/vendor/documents`
  - **ব্রুনো ফাইল:** `Vendor Portal/Store Profile & KYC/Upload KYC Document.yml`
  - **কাজ:** KYC ডকুমেন্ট আপলোড (`UploadStoreDocumentRequest` দিয়ে ভ্যালিডেট করে `StoreService` এ সেভ হয় এবং Redis ক্যাশ ফ্ল্যাশ করে)।
  - **রেসপন্স রিসোর্স:** `StoreDocumentResource`
  - **প্রত্যাশিত স্ট্যাটাস:** `201 Created`
- [x] **32. DELETE** `/api/v1/vendor/documents/:document`
  - **ব্রুনো ফাইল:** `Vendor Portal/Store Profile & KYC/Delete KYC Document.yml`
  - **কাজ:** বাতিল বা ভুল KYC ডকুমেন্ট ডিলিট করা (`StoreService` দিয়ে ডাটাবেজ এবং স্টোরেজ ডিস্ক থেকে ফিজিক্যাল ফাইল রিমুভ করে ক্যাশ ফ্ল্যাশ করে)।
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

### 📍 ধাপ ৭: ভেন্ডর ড্যাশবোর্ড ও অ্যানালিটিক্স (Dashboard & Analytics)
- [x] **33. GET** `/api/v1/vendor/dashboard`
  - **ব্রুনো ফাইল:** `Vendor Portal/Dashboard & Analytics/Get Vendor Dashboard.yml`
  - **কাজ:** ভেন্ডর ড্যাশবোর্ড মেট্রিক্স (টাইম-অ্যাওয়ার গ্রিটিংস, স্টোর প্রোফাইল, অর্ডার কাউন্টারস [New, Preparing, Ready, Done], আজকের রেভিনিউ, ৭-দিনের উইকলি সেলস চার্ট, স্টোর পারফরম্যান্স [রেটিং, সাকসেস রেট %, অ্যাভারেজ টাইম], লো-স্টক অ্যালার্ট এবং সাম্প্রতিক অর্ডার্স)। অ্যাডভান্সড ক্যাশ ট্রেইট দিয়ে এটমিক লক ও টেন্যান্ট-আইসোলেটেড ক্যাশিং দ্বারা অপ্টিমাইজড।
  - **রেসপন্স রিসোর্স:** `VendorDashboardResource`
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **34. GET** `/api/v1/vendor/analytics`
  - **ব্রুনো ফাইল:** `Vendor Portal/Dashboard & Analytics/Get Sales Analytics.yml`
  - **কাজ:** সেলস গ্রাফ অ্যানালিটিক্স (টাইমলাইন: week, month, year)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

### 📍 ধাপ ৮: প্রোডাক্ট ক্যাটালগ ও ইনভেন্টরি ম্যানেজমেন্ট (Products & Inventory)
- [ ] **35. GET** `/api/v1/vendor/products`
  - **ব্রুনো ফাইল:** `Vendor Portal/Products & Inventory/Get Store Products Catalog.yml`
  - **কাজ:** ভেন্ডরের নিজস্ব সব প্রোডাক্টের তালিকা (ইনভেন্টরি স্টক ও স্ট্যাটাস সহ)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **36. POST** `/api/v1/vendor/products`
  - **ব্রুনো ফাইল:** `Vendor Portal/Products & Inventory/Add Product (4-Step Wizard).yml`
  - **কাজ:** নতুন প্রোডাক্ট আপলোড (৪-স্টেপ উইজার্ড: বেসিক ইনফো, প্রাইসিং, ভ্যারিয়েন্টস ও ছবি)
  - **প্রত্যাশিত স্ট্যাটাস:** `201 Created`
- [ ] **37. GET** `/api/v1/vendor/products/:product`
  - **ব্রুনো ফাইল:** `Vendor Portal/Products & Inventory/Get Product Details.yml`
  - **কাজ:** নির্দিষ্ট প্রোডাক্টের বিস্তারিত ভেন্ডর ভিউ ও ভ্যারিয়েন্ট স্টক দেখা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **38. POST** `/api/v1/vendor/products/:product`
  - **ব্রুনো ফাইল:** `Vendor Portal/Products & Inventory/Update Product.yml`
  - **কাজ:** প্রোডাক্টের নাম, মূল্য, ছবি ও ভ্যারিয়েন্ট আপডেট করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **39. PATCH** `/api/v1/vendor/products/:product/stock`
  - **ব্রুনো ফাইল:** `Vendor Portal/Products & Inventory/Update Product Stock (Stepper).yml`
  - **কাজ:** দ্রুত প্রোডাক্ট বা ভ্যারিয়েন্টের স্টক সংখ্যা বাড়ানো বা কমানো
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **40. PATCH** `/api/v1/vendor/products/:product/toggle-active`
  - **ব্রুনো ফাইল:** `Vendor Portal/Products & Inventory/Toggle Product Listing Status.yml`
  - **কাজ:** প্রোডাক্ট সাময়িকভাবে মার্কেটপ্লেসে হাইড (Paused) বা অ্যাক্টিভ (Live) করা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **41. DELETE** `/api/v1/vendor/products/:product`
  - **ব্রুনো ফাইল:** `Vendor Portal/Products & Inventory/Delete Product.yml`
  - **কাজ:** প্রোডাক্ট ক্যাটালগ থেকে মুছে ফেলা (Soft delete)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---

### 📍 ধাপ ৯: ভেন্ডর অর্ডার কিউ ও ফুলফিলমেন্ট স্টেপার (Orders & Fulfillment)
- [ ] **42. GET** `/api/v1/vendor/orders`
  - **ব্রুনো ফাইল:** `Vendor Portal/Orders & Fulfillment/Get Store Orders Queue.yml`
  - **কাজ:** ইনকামিং সব অর্ডারের কিউ (ট্যাব: `new`, `preparing`, `delivered`, `cancelled`)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **43. GET** `/api/v1/vendor/orders/:order`
  - **ব্রুনো ফাইল:** `Vendor Portal/Orders & Fulfillment/Get Order Details.yml`
  - **কাজ:** নির্দিষ্ট অর্ডারের কাস্টমার ইনফো, ডেলিভারি ঠিকানা ও আইটেমস দেখা
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **44. POST** `/api/v1/vendor/orders/:order/accept`
  - **ব্রুনো ফাইল:** `Vendor Portal/Orders & Fulfillment/Accept Order.yml`
  - **কাজ:** নতুন অর্ডার এক্সেপ্ট করা (স্ট্যাটাস সরাসরি `preparing`-এ স্থানান্তরিত হয়)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **45. PATCH** `/api/v1/vendor/orders/:order/status`
  - **ব্রুনো ফাইল:** `Vendor Portal/Orders & Fulfillment/Update Fulfillment Stepper Status.yml`
  - **কাজ:** ফুলফিলমেন্ট স্টেপার আপডেট (`ready_for_pickup` ➔ `out_for_delivery` ➔ `delivered`)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`
- [ ] **46. POST** `/api/v1/vendor/orders/:order/reject`
  - **ব্রুনো ফাইল:** `Vendor Portal/Orders & Fulfillment/Reject Order.yml`
  - **কাজ:** অর্ডার রিজেক্ট করা (অটো-স্টক রিস্টোর ও স্বয়ংক্রিয় গেটওয়ে রিফান্ড)
  - **প্রত্যাশিত স্ট্যাটাস:** `200 OK`

---
> *Generated by Antigravity IDE for Rivo App Testing & Quality Assurance.*
