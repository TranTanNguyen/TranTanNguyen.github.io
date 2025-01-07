---
title: "Java Networking: Xây Dựng Ứng Dụng Mạng"
date: 2025-01-01T17:00:00+07:00
description: Tìm hiểu cách xây dựng ứng dụng mạng trong Java với các API cơ bản như Socket, ServerSocket, và DatagramSocket.
image: images/Java-Networking.png
caption: Phát triển ứng dụng mạng với Java Networking
categories:
  - programming
tags:
  - java
  - networking
  - socket
  - programming
draft: false
---

## 1. Giới thiệu về Java Networking

Java Networking cung cấp các API mạnh mẽ để giao tiếp giữa các máy tính qua mạng. Các ứng dụng mạng thường dựa trên mô hình **Client-Server**, nơi một máy chủ cung cấp dịch vụ và các máy khách kết nối để sử dụng dịch vụ đó.

### Các giao thức chính trong Java Networking:
- **TCP (Transmission Control Protocol)**: Đảm bảo truyền dữ liệu đáng tin cậy.
- **UDP (User Datagram Protocol)**: Truyền dữ liệu không đáng tin cậy nhưng nhanh.

## 2. API cơ bản trong Java Networking

### 2.1 Lớp `Socket`
Được sử dụng để giao tiếp giữa Client và Server qua TCP.

### 2.2 Lớp `ServerSocket`
Được sử dụng để tạo một máy chủ lắng nghe các kết nối từ Client.

### 2.3 Lớp `DatagramSocket` và `DatagramPacket`
Được sử dụng để truyền dữ liệu qua giao thức UDP.

---

## 3. Ứng dụng mạng đơn giản với TCP

### 3.1 Máy chủ (Server)
Máy chủ lắng nghe và phản hồi các kết nối từ Client.

```java
import java.io.*;
import java.net.*;

public class TCPServer {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(12345)) {
            System.out.println("Máy chủ đang lắng nghe trên cổng 12345...");
            
            Socket socket = serverSocket.accept(); // Chấp nhận kết nối từ client
            System.out.println("Kết nối từ: " + socket.getInetAddress());

            // Luồng đọc và ghi dữ liệu
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

            String receivedMessage;
            while ((receivedMessage = in.readLine()) != null) {
                System.out.println("Nhận: " + receivedMessage);
                out.println("Phản hồi: " + receivedMessage.toUpperCase());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
### 3.2 Máy khách (Client)
Máy khách gửi yêu cầu tới máy chủ.

```java
import java.io.*;
import java.net.*;

public class TCPClient {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 12345)) {
            System.out.println("Đã kết nối tới máy chủ!");

            // Luồng đọc và ghi dữ liệu
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader userInput = new BufferedReader(new InputStreamReader(System.in));

            String message;
            while (true) {
                System.out.print("Nhập tin nhắn: ");
                message = userInput.readLine();
                out.println(message);

                String response = in.readLine();
                System.out.println("Máy chủ trả lời: " + response);

                if ("exit".equalsIgnoreCase(message)) break;
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
## 4. Ứng dụng mạng với UDP
### 4.1 Máy chủ (UDP Server)
Máy chủ nhận và phản hồi gói tin UDP.
```java
import java.net.*;

public class UDPServer {
    public static void main(String[] args) {
        try (DatagramSocket serverSocket = new DatagramSocket(12345)) {
            byte[] receiveBuffer = new byte[1024];
            byte[] sendBuffer;

            System.out.println("UDP Server đang lắng nghe trên cổng 12345...");

            while (true) {
                DatagramPacket receivePacket = new DatagramPacket(receiveBuffer, receiveBuffer.length);
                serverSocket.receive(receivePacket); // Nhận gói tin

                String message = new String(receivePacket.getData(), 0, receivePacket.getLength());
                System.out.println("Nhận: " + message);

                String response = "Phản hồi: " + message.toUpperCase();
                sendBuffer = response.getBytes();

                InetAddress clientAddress = receivePacket.getAddress();
                int clientPort = receivePacket.getPort();

                DatagramPacket sendPacket = new DatagramPacket(sendBuffer, sendBuffer.length, clientAddress, clientPort);
                serverSocket.send(sendPacket); // Gửi phản hồi
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
### 4.2 Máy khách (UDP Client)
Máy khách gửi và nhận gói tin UDP.
```java
import java.net.*;
import java.util.Scanner;

public class UDPClient {
    public static void main(String[] args) {
        try (DatagramSocket clientSocket = new DatagramSocket()) {
            InetAddress serverAddress = InetAddress.getByName("localhost");
            byte[] sendBuffer;
            byte[] receiveBuffer = new byte[1024];

            Scanner scanner = new Scanner(System.in);

            while (true) {
                System.out.print("Nhập tin nhắn: ");
                String message = scanner.nextLine();
                sendBuffer = message.getBytes();

                DatagramPacket sendPacket = new DatagramPacket(sendBuffer, sendBuffer.length, serverAddress, 12345);
                clientSocket.send(sendPacket); // Gửi gói tin

                DatagramPacket receivePacket = new DatagramPacket(receiveBuffer, receiveBuffer.length);
                clientSocket.receive(receivePacket); // Nhận phản hồi

                String response = new String(receivePacket.getData(), 0, receivePacket.getLength());
                System.out.println("Máy chủ trả lời: " + response);

                if ("exit".equalsIgnoreCase(message)) break;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
## Kết luận
Java Networking cung cấp các API mạnh mẽ để phát triển các ứng dụng mạng. Tùy vào nhu cầu cụ thể, bạn có thể sử dụng TCP cho truyền dữ liệu đáng tin cậy hoặc UDP cho truyền dữ liệu nhanh hơn.

Hãy bắt đầu thử nghiệm và phát triển các ứng dụng mạng của bạn ngay hôm nay!