

# Multithreading in Java
-----------------------------------
## Abstract
Multithreading is a feature in Java that allows multiple threads to run simultaneously. This paper covers the basics of multithreading, how to create threads, their lifecycle, and how they can be used effectively in server applications.


## Introduction
Java's multithreading allows programs to perform several tasks at once, which can lead to better performance, especially in applications like web servers that need to handle many requests at the same time.

## Thread Creation in Java
There are two main ways to create threads in Java:

### 1. Extending the Thread Class
You can create a thread by extending the Thread class and overriding its run() method. This method contains the code that will be executed when the thread starts.

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }
}

// Creating and starting the thread
MyThread thread = new MyThread();
thread.start();
```

### 2. Implementing the Runnable Interface
Another way is to implement the Runnable interface. This approach is useful if you want your class to extend another class.

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Runnable thread is running...");
    }
}

// Creating and starting the thread
Thread thread = new Thread(new MyRunnable());
thread.start();
```

## Thread Life Cycle
A thread in Java can be in different states:
1. **New**: A thread is in this state when it is created but not yet started.
2. **Runnable**: The thread is ready to run and waiting for CPU time.
3. **Blocked**: The thread is blocked waiting for a monitor lock.
4. **Waiting**: The thread is waiting for another thread to perform an action.
5. **Timed Waiting**: The thread is waiting for another thread for a specified time.
6. **Terminated**: The thread has finished executing.

## Thread Synchronization
When multiple threads access shared resources, synchronization is needed to avoid conflicts. Java offers several ways to synchronize threads:

### Using Synchronized Methods
You can mark a method with the synchronized keyword to ensure that only one thread can execute it at a time.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

// calling
Counter counter = new Counter();

```

### Using Synchronized Blocks
For more control, you can use synchronized blocks to lock specific sections of code.

```java
class SharedResource {
    private int data = 0;

    public void updateData(int value) {
        synchronized (this) {
            data += value; // Safely update the shared resource
        }
    }

    public int getData() {
        return data;
    }
}

// calling
SharedResource resource = new SharedResource();

```

## Practical Applications
Multithreading is especially important in server development, where it helps handle multiple client requests at the same time. For example, an HTTP server can use separate threads for each incoming request.

### Example: Simple HTTP Server
Here’s a simple example of a multithreaded HTTP server:

```java
import java.io.*;
import java.net.*;

public class SimpleHttpServer {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080)) {
            while (true) {
                Socket clientSocket = serverSocket.accept();
                new Thread(new ClientHandler(clientSocket)).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

class ClientHandler implements Runnable {
    private Socket clientSocket;

    public ClientHandler(Socket socket) {
        this.clientSocket = socket;
    }

    public void run() {
        try (BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
             PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true)) {
            String requestLine = in.readLine();
            // Process the request and send a response
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Conclusion
Understanding multithreading in Java is essential for building efficient applications, particularly for servers that handle many requests. By using different ways to create threads, managing their lifecycle, and implementing synchronization, developers can create robust applications that make good use of system resources.

## References
1. Multithreading for Beginners - [FreeCodeCamp](https://www.freecodecamp.org/news/multithreading-for-beginners/)
2. HTTP Server - [YouTube](https://www.youtube.com/watch?v=FNUdLeGfShU&list=PLAuGQNR28pW56GigraPdiI0oKwcs8gglW)
3. Java Concurrency & Multithreading - [YouTube](https://www.youtube.com/watch?v=WldMTtUWqTg)
4. Java Documentation: [Java Concurrency](https://docs.oracle.com/javase/tutorial/essential/concurrency/)

---
