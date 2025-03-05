# WebSocket Server

This is a WebSocket server implementation in C++ using the **Boost.Beast** library. It provides a simple and extensible API for handling WebSocket connections, sending and receiving messages, and broadcasting messages to all connected clients.

---

## Features

- **WebSocket Protocol**: Fully compliant with the WebSocket protocol (RFC 6455).
- **Event Handling**: Supports custom event handlers for connection, message, and disconnection events.
- **Broadcasting**: Easily broadcast messages to all connected clients.
- **Asynchronous I/O**: Built on top of **Boost.Asio** for high-performance, non-blocking I/O.
- **Extensible**: Designed to be easily extended with additional functionality.

---

## Requirements

- **C++17** or later.
- **Boost Library** (version 1.70 or later).
- **Aether**
- **Aeon** (native package manager)

---

## Installation

### 1. Using Aeon Package Manager

```bash
aeon install websockets
```

That's it.
---

## Usage

### 1. Starting the Server

To start the WebSocket server, create an instance of `SocketServer` and call the `listen` method:

```cpp
#include "Aether/aether.h"
#include <websocket.h>


using namespace Aether; 
using namespace Aether::Http; // for the server
using namespace Websocket; 

int main() {
    auto app = Server(); // Create http server which manages the event handler

    auto ws = SocketServer(app); // Create the WebSocket server, passing the HTTP server

    // WebSocket event handler
    ws.on("message", [](Session session, const Message& message) {
        print("Received message: " + message);
        session->send("echo", message); // Send event "echo" and the message
    });

    ws.listen(8080); // listen for socket.
    return 0;
}
```

### 2. Handling Events

You can define custom event handlers for connection, message, and disconnection events.

#### Example: Handling Messages

```cpp
ws.on("message", [](const Websocket::Message& message) {
    std::cout << "Received message: " << message.getData() << std::endl;
});
```

#### Example: Broadcasting Messages

```cpp
ws.broadcast("chat", "Hello, everyone!");
```

### 3. Running the Server

Compile and run the server:

```bash
aeon build
aeon run
```

---

## API Reference

### `SocketServer`

#### Methods

- **`void listen(int port)`**: Starts the server and listens on the specified port.
- **`void on(const std::string& event, EventHandler handler)`**: Registers an event handler for the specified event.
- **`void emit(const std::string& event, const Message& message)`**: Sends a message to all connected clients.
- **`void broadcast(const std::string& event, const Message& message)`**: Broadcasts a message to all connected clients.

#### Events

- **`message`**: Triggered when a message is received from a client.
- **`connection`**: Triggered when a new client connects.
- **`disconnect`**: Triggered when a client disconnects.

### `WebSocketSession`

#### Methods

- **`void start()`**: Starts the WebSocket session.
- **`void send(const std::string& event, const std::string& message)`**: Sends a message to the client.
- **`void onMessage(MessageHandler handler)`**: Registers a handler for incoming messages.

---

## Example

### Server Code

```cpp
#include "Aether/aether.h"
#include <websocket.h>

// Built-in modules
using namespace Aether; // Bring aether into scope
using namespace Aether::Http; // Bring aether's built-in HTTP module into scope

// External modules
using namespace Websocket; // External module

int main() {
    print("Starting Aether server..."); // Use print directly

    // Create the HTTP server
    auto app = Server();

    // Create the WebSocket server, passing the HTTP server
    auto ws = SocketServer(app);

    // WebSocket event handler
    ws.on("message", [](Session session, const Message& message) {
        print("Received message: " + message);
        session->send("echo", message); // Send event "echo" and the message
    });

    // Start the WebSocket server on port 8080
    ws.listen(8080);

    // HTTP request handlers
    app.get("/", [](Request& req, Response& res) {
        print("Handling GET request for /"); // Use print directly
        res.send("Hello from aeon!");
    });

    app.get("/users/:id", [](auto& req, auto& res) {
        res.send("User ID: " + req.params["id"]);
    });

    app.get("/files/*path", [](auto& req, auto& res) {
        res.sendFile("/Volumes/external/Package/aether/sample/public/" + req.params["path"]);
    });

    // Start the HTTP server on port 3000
    app.run(3000);

    return 0;
}
```

---

## Debugging

### Common Issues

1. **No Messages Received**:
   - Ensure the `messageHandler_` is set in the `WebSocketSession` class.
   - Verify that the client is sending messages correctly.

2. **Connection Issues**:
   - Check that the server is running and listening on the correct port.
   - Ensure there are no firewall or network issues blocking the connection.

3. **Boost Library Not Found**:
   - Ensure Boost is installed and properly linked during compilation.

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bugfix.
3. Submit a pull request with a detailed description of your changes.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- **Boost.Beast**: For providing a powerful WebSocket implementation.
- **Boost.Asio**: For asynchronous I/O support.

---

## Contact

For questions or feedback, please open an issue on GitHub or contact the maintainer.

---

Enjoy building with the WebSocket server! 🚀