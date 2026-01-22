# 🔥 Hướng Dẫn Cấu Hình Firebase - DÙNG VĨNH VIỄN

## ✅ Checklist - Làm theo thứ tự (Chỉ mất 3 phút):

### 1️⃣ Kích hoạt Firestore Database

1. Vào: https://console.firebase.google.com/
2. Chọn project: **toolhome-c693d**
3. Menu bên trái → Click **Firestore Database**
4. Click nút **Create database**
5. **QUAN TRỌNG:** Chọn **"Start in production mode"** (KHÔNG phải test mode)
   - Nếu chọn nhầm test mode thì chỉ dùng được 30 ngày
   - Production mode + Rules bên dưới = Dùng vĩnh viễn ✅
6. Chọn location: **asia-southeast1 (Singapore)** hoặc gần nhất
7. Click **Enable**
8. Đợi vài giây để Firestore được khởi tạo

---

### 2️⃣ Cài đặt Security Rules (Dùng mãi mãi!)

Sau khi Firestore được tạo:

1. Vào tab **Rules** (bên cạnh Data)
2. Xóa hết code cũ
3. Paste code này vào:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Cho phép đọc/ghi collection invoices - KHÔNG giới hạn thời gian
    match /invoices/{document=**} {
      allow read, write: if true;
    }
  }
}
```

4. Click **Publish** để lưu

✅ **XONG! Bây giờ dùng được mãi mãi, KHÔNG giới hạn 30 ngày!**

> 💡 **Lưu ý**: Rules này cho phép tất cả mọi người đọc/ghi (phù hợp cho dùng nội bộ/cá nhân).
> Vì bạn chỉ quản lý phòng trọ của mình nên OK. Nếu muốn bảo mật hơn thì cần thêm authentication.

---

### 3️⃣ Test thử

1. Mở file `index.html` trong trình duyệt
2. Nhấn **F12** để mở Developer Console
3. Tạo một hóa đơn test
4. Xem Console:
   - ✅ Nếu thấy "Đã lưu hóa đơn vào Firebase với ID: xxx" → Thành công!
   - ❌ Nếu có lỗi → Xem message lỗi để debug

5. Kiểm tra trên Firebase Console:
   - Vào **Firestore Database** → Tab **Data**
   - Sẽ thấy collection `invoices` với các document hóa đơn

---

### 4️⃣ Xem dữ liệu đã lưu

1. Vào Firebase Console → **Firestore Database** → Tab **Data**
2. Click vào collection **invoices**
3. Sẽ thấy tất cả hóa đơn đã tạo với các field:
   - tenantName (tên phòng)
   - billingMonth (tháng)
   - year (năm)
   - roomPrice, electricCost, waterCost...
   - total (tổng tiền)
   - createdAt (thời gian tạo)

---

## 🐛 Troubleshooting - Nếu gặp lỗi:

### Lỗi: "Missing or insufficient permissions"

→ Chưa cài đặt Security Rules đúng. Làm lại Bước 2 ở trên.

### Lỗi: "Firebase chưa được khởi tạo"

→ Kiểm tra lại Firebase Config trong file index.html

### Lỗi: Network error

→ Kiểm tra kết nối internet

### Không thấy dữ liệu trong Firebase Console

→ Kiểm tra Collection name phải là "invoices" (chữ thường)

---

## 🔒 Security Rules nâng cao (Optional)

Nếu muốn bảo mật hơn sau này, dùng rules này (cần authentication):

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /invoices/{invoiceId} {
      // Chỉ cho phép user đã đăng nhập
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

Nhưng hiện tại dùng test mode là OK!

---

## 📊 Cấu trúc dữ liệu trong Firestore

```
invoices (collection)
  └── documentId (auto-generated)
      ├── tenantName: "P01"
      ├── billingMonth: 1
      ├── year: 2026
      ├── roomPrice: 2000000
      ├── electricOld: 100
      ├── electricNew: 150
      ├── electricUsed: 50
      ├── electricPrice: 3500
      ├── electricCost: 175000
      ├── waterOld: 10
      ├── waterNew: 15
      ├── waterUsed: 5
      ├── waterPrice: 7000
      ├── waterCost: 35000
      ├── otherFees: 0
      ├── otherFeesDesc: ""
      ├── notes: ""
      ├── total: 2210000
      ├── date: "22/01/2026"
      └── createdAt: "2026-01-22T10:30:45.123Z"
```

---

## ✨ Hoàn tất!

Sau khi làm xong 2 bước trên:

- ✅ Mỗi hóa đơn tạo ra sẽ tự động lưu vào Firebase
- ✅ Có thông báo thành công ở góc phải màn hình
- ✅ Xem lịch sử tất cả hóa đơn qua nút "Lịch Sử"
- ✅ Dữ liệu được lưu vĩnh viễn trên cloud

Mọi thắc mắc cứ hỏi nhé! 🚀
