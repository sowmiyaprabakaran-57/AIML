### Procedure

#### Step 1: Set up the environment
1. Install necessary tools and libraries:
   - Python (for simplicity)
   - Libraries: `socket`, `time`, `random`, `statistics`

#### Step 2: Implement UDP Streaming

```python
import socket
import time
import random
import threading

# Define constants
SERVER_IP = "127.0.0.1"
SERVER_PORT = 12345
BUFFER_SIZE = 1024

# UDP Server (Receiver)
def udp_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    server_socket.bind((SERVER_IP, SERVER_PORT))

    print("UDP Server Listening...")

    while True:
        message, client_address = server_socket.recvfrom(BUFFER_SIZE)
        print(f"Received message from {client_address}: {message}")

# UDP Client (Sender)
def udp_client():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    message_count = 0
    while message_count < 100:  # Send 100 packets
        message = f"Message {message_count}"
        delay = random.uniform(0.05, 0.15)  # simulate variable latency
        time.sleep(delay)
        client_socket.sendto(message.encode(), (SERVER_IP, SERVER_PORT))
        message_count += 1
        print(f"Sent: {message}")

# Run the UDP Server and Client
def start_udp():
    server_thread = threading.Thread(target=udp_server)
    client_thread = threading.Thread(target=udp_client)

    server_thread.start()
    client_thread.start()

    server_thread.join()
    client_thread.join()

```

#### Step 3: Implement TCP Streaming

```python
import socket
import time
import random
import threading

# Define constants
SERVER_IP = "127.0.0.1"
SERVER_PORT = 12345
BUFFER_SIZE = 1024

# TCP Server (Receiver)
def tcp_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((SERVER_IP, SERVER_PORT))
    server_socket.listen(1)
    print("TCP Server Listening...")

    connection, client_address = server_socket.accept()
    try:
        while True:
            message = connection.recv(BUFFER_SIZE)
            if not message:
                break
            print(f"Received message: {message.decode()}")
    finally:
        connection.close()

# TCP Client (Sender)
def tcp_client():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((SERVER_IP, SERVER_PORT))

    message_count = 0
    while message_count < 100:  # Send 100 packets
        message = f"Message {message_count}"
        delay = random.uniform(0.05, 0.15)  # simulate variable latency
        time.sleep(delay)
        client_socket.send(message.encode())
        message_count += 1
        print(f"Sent: {message}")

    client_socket.close()

# Run the TCP Server and Client
def start_tcp():
    server_thread = threading.Thread(target=tcp_server)
    client_thread = threading.Thread(target=tcp_client)

    server_thread.start()
    client_thread.start()

    server_thread.join()
    client_thread.join()

```

#### Step 4: Implement Latency, Jitter, and Packet Loss Measurement

To measure the performance, we will record:
- **Latency**: Time taken for a packet to travel from client to server and back (for both UDP and TCP).
- **Jitter**: Variability in the latency across multiple packets.
- **Packet Loss**: Number of packets not received by the server.

We will modify both the server and client to track these metrics.

1. **UDP Latency and Packet Loss Tracking**:

   In the **UDP Client**, each message can include a timestamp when it is sent. The server can compute the round-trip time by noting the time it receives the packet and comparing it with the send time.

```python
# UDP Client with latency measurement
def udp_client_with_metrics():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    packet_loss_count = 0
    latencies = []

    message_count = 0
    while message_count < 100:
        message = f"Message {message_count}"
        send_time = time.time()  # Timestamp when packet is sent
        delay = random.uniform(0.05, 0.15)  # simulate variable latency
        time.sleep(delay)
        client_socket.sendto(f"{send_time}:{message}".encode(), (SERVER_IP, SERVER_PORT))
        message_count += 1
        print(f"Sent: {message}")

    client_socket.close()

# UDP Server with latency measurement
def udp_server_with_metrics():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    server_socket.bind((SERVER_IP, SERVER_PORT))

    print("UDP Server Listening...")
    packet_loss_count = 0
    latencies = []

    while True:
        message, client_address = server_socket.recvfrom(BUFFER_SIZE)
        receive_time = time.time()  # Timestamp when packet is received
        message = message.decode()
        send_time, msg = message.split(":")
        send_time = float(send_time)
        round_trip_time = receive_time - send_time
        latencies.append(round_trip_time)

        print(f"Received: {msg}, RTT: {round_trip_time:.6f}s")

    # Calculate packet loss and jitter (for demonstration)
    avg_latency = sum(latencies) / len(latencies) if latencies else 0
    jitter = sum([abs(latencies[i] - latencies[i-1]) for i in range(1, len(latencies))]) / len(latencies) if latencies else 0
    print(f"Avg Latency: {avg_latency:.6f}s, Jitter: {jitter:.6f}s")
```

#### Step 5: Execute the Comparison

You will run both the UDP and TCP tests and compare the following metrics:
- **Latency**: How quickly the data travels from the client to the server.
- **Jitter**: The variation in latency over time.
- **Packet Loss**: How often data fails to arrive at the server.

For **TCP**, ensure the server handles the acknowledgment mechanism (since TCP ensures reliability), and for **UDP**, track the time difference between when packets are sent and received.

### Example Command to Start Testing

```python
if __name__ == "__main__":
    print("Starting UDP Test")
    start_udp()  # To start UDP server and client

    print("Starting TCP Test")
    start_tcp()  # To start TCP server and client
```

### Discussion on Protocol Choice for Real-Time Applications

1. **UDP**:
   - **When to use**: Preferred for real-time applications like **video streaming** or **voice calls**, where low latency is critical, and occasional packet loss can be tolerated. UDP avoids the overhead of establishing connections and handling retransmissions.
   - **Advantages**: Low latency, suitable for time-sensitive applications, and no overhead of error correction.
   - **Disadvantages**: Higher packet loss without retransmissions, no guarantee of packet order or delivery.

2. **TCP**:
   - **When to use**: Used when reliable delivery is necessary (e.g., file transfer or certain kinds of data streaming where integrity is more important than low latency).
   - **Advantages**: Reliable, ensures correct order and delivery of packets.
   - **Disadvantages**: Higher latency due to connection setup, error correction, and retransmissions, not ideal for real-time applications.

In general, for **real-time** applications where **latency** and **jitter** are critical, **UDP** is chosen. For applications where **reliable data transfer** is critical and slight delays can be tolerated, **TCP** is preferred.
