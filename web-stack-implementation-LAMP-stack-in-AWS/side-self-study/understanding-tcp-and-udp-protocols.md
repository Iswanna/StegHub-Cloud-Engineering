# Understanding TCP and UDP Protocols

## TCP (Transmission Control Protocol)
- TCP is a protocol focused on establishing connections, facilitates dependable and sequential communication between devices on a network.
- It provides error checking, flow control, and congestion control to ensure data integrity and delivery.
- TCP establishes a connection before data transfer and ensures that data packets are received in the correct order.

## UDP (User Datagram Protocol)
- UDP is a connectionless protocol used for fast and lightweight communication between devices over a network.
- It does not provide error checking, flow control, or congestion control, making it faster but less reliable compared to TCP.
- UDP is ideal for applications where speed is more important than reliability, such as real-time multimedia streaming or online gaming.

## Commonly Used Ports
- **HTTP** (Hypertext Transfer Protocol): Port 80 (TCP)
- **HTTPS** (Hypertext Transfer Protocol Secure): Port 443 (TCP)
- **SSH** (Secure Shell): Port 22 (TCP)
- **Telnet**: Port 23 (TCP)
- **FTP** (File Transfer Protocol): Port 21 (TCP)
- **SFTP** (SSH File Transfer Protocol): Port 22 (TCP)
- **TFTP** (Trivial File Transfer Protocol): Port 69 (UDP)

## Conclusion
Understanding these protocols and ports is essential for networking and systems administration, especially in DevOps, where managing and securing network communication is crucial.

