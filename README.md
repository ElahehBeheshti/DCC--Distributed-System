# DCC--Distributed-System
import socket
import threading

def receive_data(client_socket):
    while True:
        try:
            data = client_socket.recv(1024).decode()
            if not data:
                break  # Server closed the connection
            print(f"Received: {data}")
        except Exception as e:
            print(f"Error receiving data: {e}")
            break
    print("Disconnected from server.")
    client_socket.close()

def send_data(client_socket):
    while True:
        recipient_id = input("Enter recipient ID: ")
        message = input("Enter message: ")
        data = f"{recipient_id}:{message}"
        client_socket.send(data.encode())

def connect_to_server(server_ip, server_port):
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    try:
        client_socket.connect((server_ip, server_port))
        print(f"Connected to server on port {server_port}")

        # Start threads for sending and receiving data
        threading.Thread(target=receive_data, args=(client_socket,)).start()
        send_data(client_socket)
    except Exception as e:
        print(f"Connection to server failed: {e}")

if __name__ == "__main__":
    server_ip = "127.0.0.1"  # Example server IP; adjust as needed
    server_port = 8080  # Example server port; adjust as needed or make dynamic
    connect_to_server(server_ip, server_port)


