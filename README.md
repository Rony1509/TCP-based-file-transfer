# TCP File Transfer — Client & Server

A multithreaded TCP file transfer application built with **C++**, **Boost.Asio**, and **SFML**. The client provides a graphical interface with a real-time progress bar, while the server receives files and saves them locally.

---

## Features

### Client
- GUI window built with SFML
- Type a filename and click **Send** to transfer
- Real-time progress bar showing transfer percentage
- Supports sending multiple files in one session

### Server
- Accepts multiple simultaneous clients using `std::thread`
- Displays live byte-by-byte transfer progress in the console
- Prompts for a custom save name per received file
- Handles continuous transfers per connection in a loop

---

## Requirements

| Dependency | Purpose |
|------------|---------|
| C++11 or later | Language standard |
| [Boost.Asio](https://www.boost.org/) | TCP networking |
| [SFML](https://www.sfml-dev.org/) | Client GUI & graphics |
| `arial.ttf` | Font file for the client UI |

> `arial.ttf` must be placed in the **same directory** as the client executable.

---

## Project Structure

```
.
├── client.cpp       # SFML GUI client — sends files
├── server.cpp       # Console server — receives files
├── arial.ttf        # Font required by the client
└── README.md
```

---

## Build Instructions

### Linux / macOS

**Server:**
```bash
g++ -std=c++11 server.cpp -o server -lboost_system -lpthread
```

**Client:**
```bash
g++ -std=c++11 client.cpp -o client -lboost_system -lsfml-graphics -lsfml-window -lsfml-system -lpthread
```

### Windows

**Server:**
```bash
g++ -std=c++11 server.cpp -o server.exe -lboost_system -lws2_32 -lmswsock
```

**Client:**
```bash
g++ -std=c++11 client.cpp -o client.exe -lboost_system -lsfml-graphics -lsfml-window -lsfml-system -lws2_32 -lmswsock
```

> Make sure Boost and SFML include/lib paths are set correctly in your build environment.

---

## Usage

### 1. Start the Server

```bash
./server
```

```
Server listening on port 12345..
```

### 2. Start the Client

```bash
./client
```

Enter the server's IP address and port when prompted:

```
Enter server address: 192.168.1.10
Enter server port: 12345
```

A GUI window will open.

### 3. Send a File

1. Type the filename (with path if needed) in the input box
2. Click the **Send** button
3. Watch the progress bar fill to **100%**
4. On the server side, enter a custom save name or press Enter to keep the original

---

## How It Works

```
[Client]                          [Server]
   |                                  |
   |------- filename + "\n" --------> |
   |------- file_size (size_t) -----> |
   |------- file data (chunks) -----> |
   |                                  |
   | (progress bar updates live)      | (console shows bytes received)
```

| Step | Description |
|------|-------------|
| 1 | Client connects to the server via TCP |
| 2 | Client sends the filename as a newline-terminated string |
| 3 | Client sends file size as a raw `size_t` value |
| 4 | Client sends file data in 4096-byte chunks |
| 5 | Server reads chunks and writes them to disk in binary mode |
| 6 | Both sides display real-time progress |

---

## Known Limitations

- `arial.ttf` must be present alongside the client binary
- Server custom-name prompt requires manual console interaction
- No TLS/encryption — do not use over untrusted networks
- Files are silently overwritten if the same name already exists on the server
- Backspace handling in the client input may behave unexpectedly with non-ASCII characters

---

## License

This project is provided as-is for educational and personal use.
