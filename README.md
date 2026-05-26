# Blackjack Network Project 

A complete networked multiplayer Blackjack game built in Python, featuring custom protocol design with reliable TCP communication and UDP-based server discovery. This project demonstrates proficiency in socket programming, concurrent multi-threaded environments, and client-server architecture.

## Key Features

* **Custom Binary Protocol**: Designed a custom binary message format (using magic cookies, message types, and structured byte payloads) to handle game states securely and efficiently.
* **UDP Server Discovery**: The server continuously broadcasts its availability over UDP, allowing clients to seamlessly discover and connect to the server without hardcoded IP addresses.
* **Reliable TCP Gameplay**: Game logic, card dealing, and player decisions are transmitted over a reliable TCP connection to ensure synchronization.
* **Concurrent Server**: The server utilizes threading to handle multiple clients concurrently, allowing multiple separate games/tables to run simultaneously.
* **Object-Oriented Game Logic**: Implements standard Blackjack rules (Hit, Stand, Dealer rules, win/loss/tie conditions) using clean, modular OOP design (`deck`, `card`, `table`, `dealer`, `player`).

##  Architecture Overview

* **`server/`**: Contains the core game logic (`deck.py`, `table.py`, `dealer.py`, `player.py`), the game manager, and the server-side networking components (`run_server.py`, `UdpMan.py`).
* **`client/`**: Manages the user interface (`view.py`), the client application flow (`clientFlow.py`), and the logic for parsing and playing the game state (`play.py`).
* **`network_module/`**: Contains the protocol specification (`msg_format.py`) and TCP/UDP networking utilities utilized by both the client and server.

## How to Run

Make sure you have Python 3.x installed.

### 1. Starting the Server
Run the server to begin listening for clients and broadcasting UDP offers. From the project root directory:
```bash
python -m server.run_server
```

### 2. Starting the Client
Run the client. It will automatically listen for UDP broadcasts to find an active server, then connect via TCP. From the project root directory:
```bash
python -m client.clientFlow
```

## Protocol Details

The `msg_format.py` defines a strict byte-level communication protocol. Each packet begins with a 4-byte Magic Cookie (`0xabcddcba`) for validation.

* **Offer (UDP)**: Server broadcasts presence (`0x2`). Includes TCP port and Server Name.
* **Request (TCP)**: Client responds to an offer (`0x3`). Specifies team name and the desired number of rounds.
* **Client Payload (TCP)**: Client decision to 'Hit' or 'Stand' (`0x4`).
* **Server Payload (TCP)**: Server sends card data (suit, rank) and round results (`0x5`).
