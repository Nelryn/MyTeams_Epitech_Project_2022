# MyTeams Epitech Project 2022

> **Collaborative communication application (server + client)**
> Inspired by Microsoft Teams®, built entirely in **C** using **TCP sockets**.
> Groupd of 3, 2nd year students.

---

## 🧩 Overview

**MyTeams** is a **client-server communication platform** that allows multiple users to communicate, organize, and collaborate in real-time through a terminal-based interface.

The project was developed focusing on socket programming, I/O multiplexing with `select()`, and protocol design.

The system consists of two programs:

* 🖥️ **`myteams_server`** — handles multiple clients, teams, channels, threads, and data persistence.
* 💻 **`myteams_cli`** — a command-line client that interacts with the server using a custom protocol.

---

## ⚙️ Compilation

Compile both the server and client using the provided **Makefile**:

```bash
make
```

### Binaries produced:

* `myteams_server`
* `myteams_cli`

Each target includes standard Makefile rules:

```
make        # build the project
make clean  # remove object files
make fclean # remove binaries
make re     # rebuild everything
```

---

## 🚀 Usage

### 🖥️ Server

```bash
./myteams_server <port>
```

**Description:**
Starts the server on the specified port, handling multiple simultaneous client connections via `select()`.

On shutdown, the server **saves all data** (users, teams, messages, etc.) to disk.
On restart, it automatically **loads** any existing saved data.

**Example:**

```bash
./myteams_server 8080
```

---

### 💻 Client

```bash
./myteams_cli <ip> <port>
```

**Description:**
Connects the client to a running `myteams_server` instance via TCP.

**Example:**

```bash
./myteams_cli 127.0.0.1 8080
```

---

## 📚 Features

### 🖥️ Server-Side

* Handles multiple clients simultaneously using `select()`
* No use of `fork()` or threads
* Supports:

  * User creation and management
  * Team creation/join/leave
  * Channels and threads within teams
  * Comments within threads
  * Private user-to-user messaging
* Saves and restores all data (users, teams, channels, threads, messages)
* Uses provided **logging library** (`libs/myteams`) for all event output
* Ensures proper security:

  * Unauthenticated users cannot access or see others’ data
  * Only subscribed users can access team-specific content

---

### 💻 Client-Side (CLI)

The client accepts commands directly from **stdin**, communicating with the server via your custom protocol.

#### Core Commands

| Command                            | Description                               |
| ---------------------------------- | ----------------------------------------- |
| `/help`                            | Display available commands                |
| `/login "user_name"`               | Log in with a username                    |
| `/logout`                          | Disconnect from the server                |
| `/users`                           | List all registered users                 |
| `/user "user_uuid"`                | Display user information                  |
| `/send "user_uuid" "message_body"` | Send a private message                    |
| `/messages "user_uuid"`            | List all messages exchanged with a user   |
| `/subscribe "team_uuid"`           | Subscribe to a team and its events        |
| `/subscribed`                      | List all teams you’re subscribed to       |
| `/subscribed "team_uuid"`          | List all users subscribed to a team       |
| `/unsubscribe "team_uuid"`         | Unsubscribe from a team                   |
| `/use`                             | Set current context (team/channel/thread) |
| `/create`                          | Create resources depending on context     |
| `/list`                            | List resources depending on context       |
| `/info`                            | Show information about current context    |

---

### 🏗️ Context-Sensitive Commands

#### `/create`

| Context          | Command                                        | Description          |
| ---------------- | ---------------------------------------------- | -------------------- |
| None             | `/create "team_name" "team_description"`       | Create a new team    |
| Team selected    | `/create "channel_name" "channel_description"` | Create a new channel |
| Channel selected | `/create "thread_title" "thread_message"`      | Create a new thread  |
| Thread selected  | `/create "comment_body"`                       | Create a new reply   |

#### `/list`

| Context | Description                    |
| ------- | ------------------------------ |
| None    | List all teams                 |
| Team    | List all channels in that team |
| Channel | List all threads               |
| Thread  | List all replies               |

#### `/info`

| Context | Description                  |
| ------- | ---------------------------- |
| None    | Display current user info    |
| Team    | Display current team info    |
| Channel | Display current channel info |
| Thread  | Display current thread info  |

---

## 🧠 Data Model Constraints

| Field        | Max Length     |
| ------------ | -------------- |
| Name         | 32 characters  |
| Description  | 255 characters |
| Message Body | 512 characters |

All arguments must be **quoted with double quotes (`"`)**.
Missing or mismatched quotes should be treated as **input errors**.

---

## 🔐 Security Rules

* Users not logged in cannot:
  * View connected users
  * Subscribe, list, or create teams/channels
    
* Users not subscribed to a team:
  * Cannot create or read content within it
  * Will not receive event notifications from that team

---

## 🧩 Project Structure

```
MyTeams_Epitech_Project_2022/
├── include/               # Header files
├── client/                # Client-side source files
├── server/                # Server-side source files
├── utils/                 #  Utility files
├── test/                  #  Unit tests
├── libs/                  # Provided logging library
├── Makefile               # Build rules
├── README.md              # Documentation
```
