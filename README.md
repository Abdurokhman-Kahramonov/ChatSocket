# 💬 Simple ChatSocket (Java Client–Server Chat Application)

A lightweight console-based **real-time chat system** built using **Java Sockets** and **Threads**.  
It consists of two main parts:
- **Server** — handles client connections and message broadcasting.  
- **Client** — connects to the server and sends/receives messages concurrently.

---

## 🧭 Overview

This project demonstrates the fundamentals of:
- **Network programming** with `Socket` and `ServerSocket`
- **Multithreading** to handle multiple clients simultaneously
- **I/O Streams** for communication between clients
- **Simple broadcast messaging** (every client sees all messages)

---

## 🧱 Technologies Used

- **Java SE 8+**  
- **Sockets API** (`java.net`)  
- **Input/Output Streams** (`java.io`)  
- **Threads** (`java.lang.Thread` and `Runnable`)  
- **Collections Framework** (`ArrayList`)

---

## 🗂️ Project Structure

```
src/
 └── Server/
      ├── Server.java
      └── Client.java
 └── Server.iml
```

---

## 🖥️ Server Side

### `Server.java`

Handles all network connections and broadcasts messages to connected clients.

#### Key Responsibilities
- Opens a `ServerSocket` on a specified port (default: **12345**)
- Accepts new client connections
- Stores all active clients’ output streams in a list
- Starts a new `ClientHandler` thread for each client
- Relays received messages to all clients

#### Example Flow
```java
public static void main(String[] args) throws IOException {
    new Server(12345).run();
}
```

**Console Output Example**
```
Port 12345 is now open.
Connection established with client: 127.0.0.1
Connection established with client: 192.168.0.5
```

---

### `ClientHandler.java` (inner class)
Handles messages coming from each connected client.

**Responsibilities:**
- Listens for incoming text from a single client.
- Passes every message to the server for broadcasting.

```java
@Override
public void run() {
    String message;
    Scanner sc = new Scanner(this.client);
    while (sc.hasNextLine()) {
        message = sc.nextLine();
        server.broadcastMessages(message);
    }
    sc.close();
}
```

---

## 💻 Client Side

### `Client.java`
Connects to the server and allows users to chat in real time.

#### How It Works
1. Connects to the server via IP and port.
2. Starts a background thread to **receive messages** (`ReceivedMessagesHandler`).
3. Prompts the user to **enter a nickname**.
4. Sends messages prefixed with the nickname to the server.

```java
Socket client = new Socket(host, port);
System.out.println("Client successfully connected to server!");
```

**Example Usage**
```
Enter a nickname: Abdu
Send messages:
Abdu: Hello everyone!
```

---

### `ReceivedMessagesHandler.java` (inner class)
Runs in a background thread to continuously print messages coming from the server.

```java
public void run() {
    Scanner s = new Scanner(server);
    while (s.hasNextLine()) {
        System.out.println(s.nextLine());
    }
    s.close();
}
```

This allows the client to both **send** and **receive** messages at the same time.

---

## ⚙️ How the Chat Works

```
+-------------+      +------------------+      +-------------+
|   Client A  | <--> |   ServerSocket   | <--> |   Client B  |
+-------------+      +------------------+      +-------------+
        ↘                   ↕                       ↙
                Broadcast to All Clients
```

- Each connected client runs its own **listener thread**.
- The **server** maintains a shared list of output streams.
- Any message from one client is sent to **all clients** instantly.

---

## 🚀 Running the Application

### 1. Compile All Files
```bash
javac Server/*.java
```

### 2. Start the Server
```bash
java Server.Server
```

Output:
```
Port 12345 is now open.
```

### 3. Start One or More Clients
Open new terminals and run:
```bash
java Server.Client
```

Each client will connect and start chatting instantly.

---

## 💡 Example Chat Session

**Terminal 1 (Server)**
```
Port 12345 is now open.
Connection established with client: 127.0.0.1
Connection established with client: 127.0.0.1
```

**Terminal 2 (Client A)**
```
Enter a nickname: Alice
Send messages:
Alice: Hello everyone!
Bob: Hi Alice!
```

**Terminal 3 (Client B)**
```
Enter a nickname: Bob
Send messages:
Bob: Hi Alice!
```

---

## ⚡ Core Concepts Illustrated

| Concept | Description |
|----------|--------------|
| `ServerSocket` | Listens for incoming TCP connections |
| `Socket` | Represents a TCP connection between client and server |
| `Thread` | Handles simultaneous clients |
| `PrintStream` | Sends text messages to clients |
| `Scanner` | Reads input from sockets |
| `List<PrintStream>` | Stores client outputs for broadcast |

---

## 🧩 Possible Improvements

- Add client **disconnect detection** and removal from list.  
- Support for **private messages** (e.g., `/msg username text`).  
- Implement **GUI** using `Swing` or `JavaFX`.  
- Add **timestamped messages**.  
- Encrypt messages for secure chat.  

---

## 📜 License

This project is open-source and free to use for educational purposes.
