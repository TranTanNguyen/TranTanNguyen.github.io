---
title: 'Xử lý bất đồng bộ trong Javacript - Phần 2'
date: 2024-12-21
description: Bài này chúng ta sẽ tìm hiểu cách xử lý bất đồng bộ trong Javacript.
image: images/bat-dong-bo2.png
draft: false
tags:
  - demo
  - shortcode
---
# Xử lý bất đồng bộ trong Javascript - Phần 2



## 3.3 Async/await (ES7)

Async/await là cơ chế giúp bạn thực thi các thao tác bất đồng bộ một cách tuần tự hơn, giúp đoạn code nhìn qua tưởng như đồng bộ nhưng thực ra lại là chạy bất đồng bộ, làm cho việc đọc và viết mã trở nên dễ hiểu hơn.

### 3.3.1 Async

Để định nghĩa một hàm bất đồng bộ với `async`, ta cần khai báo từ khóa `async` ngay trước từ khóa định nghĩa hàm.

#### Regular function:
```javascript
async function getUser() {
    return ...;
}
```

#### Function expression:
```javascript
getUser = async function() {
    return ...;
};
```

#### Kết hợp với cú pháp arrow function của ES6:
```javascript
getUser = async () => { 
    ... 
};
```

Giá trị trả về của một hàm `async` sẽ luôn là một **Promise** mặc cho bạn có gọi `await` hay không. Nếu trong code không trả về `Promise` nào thì sẽ có một `Promise` mới được resolve với giá trị lúc đầu (nếu không có giá trị nào trong `return`, kết quả trả về sẽ là `undefined`).

- **Promise thành công:** Kết quả được trả về qua từ khóa `return` của hàm `async`.
- **Promise thất bại:** Kết quả được đẩy qua từ khóa `throw` trong hàm `async`.

### 3.3.2 Await

`await` giúp cú pháp trông dễ hiểu hơn. Thay vì phải dùng `.then()` nối tiếp nhau, chỉ cần đặt `await` trước mỗi hàm mà chúng ta muốn đợi kết quả của thao tác bất đồng bộ. **Lưu ý:** `await` chỉ dùng được trong hàm có `async` đứng phía trước.

#### Ví dụ sử dụng Async/Await:
```javascript
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log('Dữ liệu:', data);
    } catch (error) {
        console.error('Lỗi:', error);
    }
}

fetchData();
```

Trong đoạn code trên:
1. Hàm `fetchData` được định nghĩa với từ khóa `async`.
2. `await` được sử dụng để đợi kết quả của `fetch` và `response.json()` trước khi thực hiện các dòng lệnh tiếp theo.
3. Sử dụng `try-catch` để xử lý lỗi trong quá trình bất đồng bộ.

---

## Tài liệu tham khảo:

- [Async/Await hỗ trợ Javascript code đồng bộ đơn giản](https://freelancervietnam.vn/async-await-ho-tro-javascript-code-dong-bo-don-gian/)
- [6 lý do async/await đánh bại Promises](https://techmaster.vn/posts/34304/6-ly-do-asyncawait-danh-bai-promises)
- [Javascript: Tìm hiểu về Promise](https://toidicodedao.com/2016/07/05/javascript-promise/)
