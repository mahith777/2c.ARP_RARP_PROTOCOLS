# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
server.py
```
import socket

# ARP table: IP -> MAC
arp_table = {
    "192.168.1.1": "4A:30:10:21:10:1A",
    "192.168.1.2": "5E:12:3B:44:AA:09",
    "192.168.1.3": "6C:77:21:9D:E0:5F",
    "192.168.1.4": "7D:08:C3:15:B2:44"
}

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("127.0.0.1", 8000))
server.listen(5)
print("ARP Server started. Waiting for client...")

while True:
    conn, addr = server.accept()
    print("Connected with", addr)

    ip = conn.recv(1024).decode().strip()
    print("Request received for IP:", ip)

    mac = arp_table.get(ip, "MAC address not found")
    conn.send(mac.encode())
    print("Reply sent:", mac)

    conn.close()
```
client.py
```
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("127.0.0.1", 8000))

ip = input("Enter the IP address: ")
client.send(ip.encode())

mac = client.recv(1024).decode()
print("MAC address of", ip, "is:", mac)

client.close()
```

## OUPUT - ARP
## Client:
<img width="1266" height="140" alt="image" src="https://github.com/user-attachments/assets/afc6dd67-8d12-48a2-9290-9113a8847980" />

## Server:
<img width="1423" height="201" alt="image" src="https://github.com/user-attachments/assets/3067ff5a-2ab7-4cc5-ae47-c13a7c1bdd14" />

## PROGRAM - RARP
client.py
```
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("127.0.0.1", 9000))

mac = input("Enter the MAC address: ")
client.send(mac.encode())

ip = client.recv(1024).decode()
print("IP address of", mac, "is:", ip)

client.close()
```

server.py
```
import socket

# RARP table: MAC -> IP
rarp_table = {
    "4A:30:10:21:10:1A": "192.168.1.1",
    "5E:12:3B:44:AA:09": "192.168.1.2",
    "6C:77:21:9D:E0:5F": "192.168.1.3",
    "7D:08:C3:15:B2:44": "192.168.1.4"
}

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("127.0.0.1", 9000))
server.listen(5)
print("RARP Server started. Waiting for client...")

conn, addr = server.accept()
print("Connected with", addr)

mac = conn.recv(1024).decode().upper()
print("Request received for MAC:", mac)

ip = rarp_table.get(mac, "IP address not found")
conn.send(ip.encode())

conn.close()
server.close()
```

## OUPUT -RARP
<img width="1920" height="1080" alt="Screenshot (68)" src="https://github.com/user-attachments/assets/e82a2caa-dd65-4134-8df3-4c59d8959cd7" />


## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
