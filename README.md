# stream-p2p-codec

A peer-to-peer live streaming application utilizing a centralized architecture. The system operates on an unstructured network topology and is implemented in C++ using raw sockets with the `pthread` library for multithreading. 

**stream-p2p-codec** combines a peer-to-peer paradigm with a client-server model, enabling each peer to function as both a client and a server.

---

## Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/J268sing/stream-p2p-codec.git
   cd ./stream-p2p-codec
   make
   ```

## Prerequisites

Ensure the following libraries are installed before building:
- **libvlc**
- **SFML**
- **pthread**

---

## Usage

### Start the Central Server
1. Start the central server (the backbone of the architecture):
```sh
./central <Your_IP/Server_IP>
```
2. Start a peer (acts as both client and server):
```sh
./server <Your_IP>
./client <Central_Server_IP>
```
3. ## Note
- File-to-IP mapping (file name → IP addresses of peers) must currently be entered manually in a space-separated format.

---

## Architecture

### System Model
- A central server handles file requests from peers by mapping file names to the IP addresses of peers storing those files.
- When a peer requests a file, it connects to the provided peer IPs and downloads the file.
- Each peer can simultaneously serve multiple other peers and download files for its own streaming needs.

**Note**: Files intended for sharing by a peer must be placed in its `server` directory.

---

## Key Classes

### **Connection Class**
- Handles connections between peers and the central server or between two peers.
- Creates connection objects using the peer's IP address and port.
- Supports both client and server functionalities for a peer, enabling simultaneous connections across multiple threads.

### **Data Class**
- Manages data transmission (e.g., file or string transfers) as byte streams.  
- Includes the following derived classes:
  - **BufferData**: Handles strings or queries sent as byte arrays.
    ```cpp
    Data* dataObj = new BufferData(char_array, size_of_array);
    ```
  - **FileData**: Manages file reads or writes.
    ```cpp
    Data* dataObj = new FileData(file_name, FileData::FTYPE::READ);
    ```
- The `Connection` class processes all data objects, ensuring compatibility with both derived classes.

---

## UML Diagram

![UML Diagram](http://i.imgur.com/EI1FBSZ.jpg?1)

## Implementation

- Implemented using raw C++ sockets.
- Multithreading is achieved via the `pthread` library.
- Uses `libvlc` for processing received multimedia data.
- The interface is rendered using `SFML`.

---

## Demo

1. **Request by one peer** with the IP of the central server (file request):  
   ![Client Request](http://i.imgur.com/9j3mOpq.png?1)

2. **Response by the central server** with a list of IPs having the file:  
   ![Central Server Response](http://i.imgur.com/pmTBVME.png?1)

3. **Response by another peer** providing the file:  
   ![Peer Response](http://i.imgur.com/slT6joq.png?1)

4. **Streaming video** in the SFML player:  
   ![Streaming Video](http://i.imgur.com/dR3yNF5.png)

The system successfully demonstrated peers serving multiple requests while downloading packets simultaneously.

---

## Contribution

Feel free to file issues and submit pull requests—contributions are welcome.

---

## License

This project is licensed under the **MIT License**.
