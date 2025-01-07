---
title: "Tìm Hiểu Array Trong Java: Khái Niệm và Thực Hành"
date: 2025-01-01
description: Khám phá cách sử dụng mảng (array) trong Java để lưu trữ và quản lý dữ liệu hiệu quả, cùng các ví dụ minh họa.
image: images/array.png
caption: "Học cách sử dụng Array trong Java thông qua ví dụ thực tiễn"
categories:
  - programming
tags:
  - java
  - array
  - data-structure
  - programming
draft: false
---

## 1. Giới thiệu về Array trong Java

**Array (mảng)** là một cấu trúc dữ liệu trong Java được sử dụng để lưu trữ nhiều giá trị cùng kiểu trong một biến duy nhất. Mỗi phần tử trong mảng có một chỉ số (index) bắt đầu từ 0.

### Đặc điểm chính của Array:
- **Kích thước cố định**: Kích thước của mảng được xác định khi khai báo.
- **Truy cập nhanh**: Truy cập phần tử bằng chỉ số.
- **Lưu trữ đồng nhất**: Chỉ lưu trữ các phần tử cùng kiểu dữ liệu.

---

## 2. Cách khai báo và khởi tạo Array

### 2.1 Khai báo mảng

```java
// Khai báo mảng
int[] numbers; // Mảng số nguyên
String[] names; // Mảng chuỗi
### 2.2 Khởi tạo mảng
```java
// Cách 1: Khởi tạo mảng với kích thước cố định
int[] numbers = new int[5]; // Mảng có 5 phần tử

// Cách 2: Khởi tạo mảng với giá trị sẵn có
int[] scores = {90, 85, 70, 80, 95};

// Cách 3: Khởi tạo và gán giá trị từng phần tử
numbers[0] = 10;
numbers[1] = 20;
```
## 3. Các thao tác cơ bản với mảng
### 3.1 Truy cập phần tử trong mảng
Sử dụng chỉ số để truy cập và thay đổi giá trị của phần tử.
```java
public class ArrayAccess {
    public static void main(String[] args) {
        int[] scores = {90, 85, 70, 80, 95};

        // Truy cập phần tử
        System.out.println("Phần tử đầu tiên: " + scores[0]);

        // Thay đổi giá trị
        scores[2] = 75;
        System.out.println("Phần tử thứ ba sau khi thay đổi: " + scores[2]);
    }
}
```
### 3.2 Duyệt mảng
**Sử dụng vòng lặp for**
**Sử dụng vòng lặp for-each**
```java
public class ForEachLoop {
    public static void main(String[] args) {
        String[] names = {"Alice", "Bob", "Charlie"};

        for (String name : names) {
            System.out.println("Tên: " + name);
        }
    }
}
```
## 4. Mảng nhiều chiều (Multidimensional Array)
### 4.1 Khai báo và khởi tạo mảng hai chiều
```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```
### 4.2 Truy cập phần tử trong mảng hai chiều
```java
public class MultiDimensionalArray {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };

        System.out.println("Phần tử tại (1, 2): " + matrix[0][1]); // 2
    }
}
```
### 4.3 Duyệt mảng hai chiều
```java
public class Array2D {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```
## 5. Các lỗi thường gặp khi làm việc với mảng
**IndexOutOfBoundsException**: Xảy ra khi truy cập chỉ số nằm ngoài phạm vi mảng.
```java
int[] numbers = {1, 2, 3};
System.out.println(numbers[3]); // Lỗi vì mảng chỉ có 3 phần tử (index từ 0 đến 2)
```
**NullPointerException**: Gọi phương thức hoặc truy cập phần tử trên một mảng chưa được khởi tạo.
```java
int[] numbers = null;
System.out.println(numbers.length); // Lỗi NullPointerException
```
## 6. So sánh Array với Collections
|Tiêu chí|	Array|	Collections|
|--------|-------|-------------|
|**Kích thước**|	Cố định|	Linh hoạt|
|**Các phần tử**|	Cùng kiểu dữ liệu|	Hỗ trợ kiểu dữ liệu khác nhau|
|**Hiệu suất**|	Tốt hơn trong trường hợp cố định|	Chậm hơn do phải xử lý linh hoạt|
|**Các thao tác nâng cao**|	Hạn chế|	Hỗ trợ tìm kiếm, sắp xếp, xóa
## 7. Kết luận
Mảng (Array) là một cấu trúc dữ liệu cơ bản nhưng mạnh mẽ trong Java. Việc hiểu cách khai báo, khởi tạo và thao tác với mảng sẽ giúp bạn xây dựng các chương trình hiệu quả hơn. Tuy nhiên, khi ứng dụng đòi hỏi tính linh hoạt, bạn nên cân nhắc sử dụng Java Collections Framework thay vì mảng truyền thống.