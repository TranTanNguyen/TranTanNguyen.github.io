---
title: 'Xử lý bất đồng bộ trong Javacript - Phần 1'
date: 2024-12-21
description: Bài này chúng ta sẽ tìm hiểu cách xử lý bất đồng bộ trong Javacript.
image: images/bat-dong-bo.png
draft: false
tags:
  - demo
  - shortcode
---
# Xử Lý Bất Đồng Bộ Trong JavaScript - Phần 1
**Chắc chắn khi lập trình, bạn sẽ có các công việc cần thời gian delay (gọi API, lấy dữ liệu từ Database, đọc/ghi file,...). Và đây chính là lúc xử lý bất đồng bộ lên ngôi, hãy cùng mình tìm hiểu về bất đồng bộ trong Javascript và chúng giúp code dễ dàng hơn thế nào nhé.**

Trong phần đầu tiên này, chúng ta sẽ cùng tìm hiểu và khái niệm cũng như một số phương án xử lý hay dùng.
## 1. Quá Trình Đồng Bộ (Synchronous)
Quá trình đồng bộ thực hiện các câu lệnh theo thứ tự từ trên xuống dưới, mỗi câu lệnh phải hoàn thành trước khi câu lệnh tiếp theo bắt đầu.

### Ví dụ:
```javascript
console.log("job1");
console.log("job2");
console.log("job3");
```
#### Kết quả:
```
job1
job2
job3
```
#### Ưu điểm:
- Dễ kiểm soát.
- Lỗi xảy ra sẽ ngăn chặn toàn bộ chương trình tiếp tục.

#### Hạn chế:
- Hiệu năng thấp khi xử lý các tác vụ tốn thời gian (đọc file, gọi API,...) vì chương trình phải chờ.

---

## 2. Quá Trình Bất Đồng Bộ (Asynchronous)
Các câu lệnh có thể chạy song song và không cần chờ câu lệnh trước đó hoàn thành.

### Ưu điểm:
- Tối ưu thời gian xử lý.
- Không làm gián đoạn luồng chính của chương trình.

### Hạn chế:
- Kết quả có thể không theo thứ tự.
- Khó kiểm soát và debug.

---

## 3. Các Cách Xử Lý Bất Đồng Bộ Phổ Biến

### 3.1 Sử dụng Callback (ES5)
Callback là hàm được truyền vào một hàm khác dưới dạng tham số và sẽ được thực thi khi hàm đó hoàn thành.

#### Ví dụ:
```javascript
function asyncFunction(callback) {
   console.log("Start");
   setTimeout(() => {
      callback();
   }, 1000);
   console.log("Waiting");
}

let printEnd = function() {
   console.log("End");
}

asyncFunction(printEnd);
```
#### Kết quả:
```
Start
Waiting
End
```
#### Hạn chế:
- Gặp vấn đề Callback Hell khi lồng nhiều callback:

```javascript
function getData(link, callback) {
   setTimeout(() => {
      callback();
   }, 1000);
}

getData("Data1", () => {
   getData("Data2", () => {
      getData("Data3", () => {
         console.log("Done");
      });
   });
});
```

### 3.2 Sử dụng Promise (ES6)
Promise là một "lời hứa" đại diện cho kết quả của một tác vụ bất đồng bộ, được thực hiện trong tương lai.

#### Tạo Promise:
```javascript
let promise = new Promise((resolve, reject) => {
  // Code bất đồng bộ
});
```
- **`resolve`**: Gọi khi thành công.
- **`reject`**: Gọi khi lỗi xảy ra.

#### Ví dụ:
```javascript
const randomNumber = new Promise((resolve, reject) => {
   const url = 'https://www.random.org/integers/?num=1&min=1&max=10&col=1&base=10&format=plain&rnd=new';
   let request = new XMLHttpRequest();

   request.open('GET', url);
   request.onload = function() {
      if (request.status === 200) {
         resolve(request.response);
      } else {
         reject(Error(request.statusText));
      }
   };

   request.onerror = function() {
      reject(Error('Error fetching data.'));
   };

   request.send();
});

randomNumber
   .then((res) => {
      console.log("Success:", res);
   })
   .catch((err) => {
      console.log("Error:", err.message);
   });
```

#### Promise Chaining:
```javascript
getAsyncData(url)
   .then((res) => getAsyncData(res.url1))
   .then((res) => getAsyncData(res.url2))
   .then((res) => getAsyncData(res.url3))
   .then((res) => {
      console.log("Done", res);
   });
```
---

### Tạm Kết
- Callback và Promise giúp xử lý bất đồng bộ hiệu quả hơn.
- Promise giải quyết vấn đề Callback Hell.
- Trong phần 2, chúng ta sẽ tìm hiểu về Async/Await!
