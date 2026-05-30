# Chapter 03: MCP Architecture

> Understanding the core components of Model Context Protocol (MCP) and how they communicate with each other.

---

# Architecture Diagram

![MCP Architecture](images/MCP%20Archtecture.png)

> High-level architecture showing the Host, Clients, Local MCP Servers, Remote MCP Servers, and communication protocols.

---

# Diagram Overview

The diagram shows how a user interacts with an AI application (Host), which uses MCP Clients to communicate with different MCP Servers.

There are two types of servers shown:

### Local MCP Server

Runs on the same machine as the Host.

Example:
- Filesystem Server

Communication:

```text
JSON-RPC 2.0 using STDIO
```

Capabilities:
- Tools
- Resources
- Prompts

---

### Remote MCP Server

Runs on a different machine and is accessed through the network.

Example:
- GitHub Server

Communication:

```text
JSON-RPC 2.0 using HTTP + SSE
```

Capabilities:
- Tools
- Resources
- Prompts

---

# Communication Flow

```text
User
 ↓
Host
 ↓
Client
 ↓
MCP Server
 ↓
Tools / Resources / Prompts
 ↓
Response
 ↓
Host
 ↓
User
```

---

# Key Components Visible In The Diagram

| Component | Purpose |
|------------|----------|
| User | Interacts with the AI application |
| Host | Main AI application (Claude Desktop, Cursor, etc.) |
| Client | Connects Host to MCP Servers |
| Local Server | Runs on the same machine |
| Remote Server | Runs over the network |
| Tools | Actions AI can perform |
| Resources | Data AI can read |
| Prompts | Reusable instruction templates |
| JSON-RPC | Standard communication protocol |
| STDIO | Communication with local servers |
| HTTP + SSE | Communication with remote servers |

---

# Learning Objectives

After completing this chapter, you should be able to:

* Understand the core components of MCP.
* Explain Host, Client, and Server roles.
* Understand local and remote MCP servers.
* Explain how communication occurs.
* Understand Tools, Resources, and Prompts.
* Explain the architecture in interviews.

---

# 1. High-Level Architecture

The MCP ecosystem consists of three major components:

```text
Host
 ↓
Client
 ↓
MCP Server
```

Each component has a specific responsibility.

---

# 2. Complete Architecture Flow

```text
User
 ↓
Host
 ↓
Client
 ↓
MCP Server
 ↓
Tools / Resources / Prompts
```

The user interacts with the host.

The host uses clients to communicate with MCP servers.

The servers provide capabilities through tools, resources, and prompts.

---

# 3. User

The user is the person interacting with the AI application.

Examples:

* Developer
* Data Scientist
* Student
* Researcher

Example request:

```text
Read my project files and summarize them.
```

The user does not communicate directly with MCP servers.

---

# 4. Host

The Host is the application where the AI model runs.

Examples:

* Claude Desktop
* Cursor
* Windsurf
* Open WebUI

The host acts as the central controller.

---

## Responsibilities of Host

### User Interaction

Receives requests from users.

### AI Interaction

Sends requests to the AI model.

### MCP Management

Manages connections to MCP servers.

### Client Management

Creates and controls MCP clients.

---

## Example

```text
User
 ↓
Claude Desktop (Host)
```

---

# 5. MCP Client

The client is responsible for communicating with MCP servers.

Think of the client as a messenger.

---

## Responsibilities

### Server Discovery

Find available MCP servers.

### Connection Management

Create and maintain server connections.

### Request Handling

Send requests.

### Response Handling

Receive responses.

---

## Important Point

A single host can have multiple clients.

Example:

```text
Host
 ├── Client 1
 ├── Client 2
 └── Client 3
```

Each client can communicate with different MCP servers.

---

# 6. MCP Server

The MCP Server provides capabilities to AI systems.

The server exposes:

* Tools
* Resources
* Prompts

---

## Responsibilities

### Capability Exposure

Provide available functionality.

### Request Processing

Handle requests from clients.

### Response Generation

Return results.

---

## Examples

### Filesystem Server

Provides file operations.

### GitHub Server

Provides repository operations.

### Database Server

Provides database access.

### Notion Server

Provides workspace access.

---

# 7. Local MCP Server

A local server runs on the same machine.

Example:

```text
Your Laptop
 ├── Claude Desktop
 └── Filesystem MCP Server
```

---

## Advantages

### Faster Communication

No internet required.

### Better Privacy

Data stays on your machine.

### Lower Latency

Quick responses.

---

## Example

Filesystem Server

Capabilities:

```text
Read File
Write File
Delete File
Create Folder
```

---

# 8. Remote MCP Server

A remote server runs on another machine.

Usually accessed through the internet.

Example:

```text
Your Computer
      ↓
Internet
      ↓
GitHub MCP Server
```

---

## Advantages

### Centralized Access

Shared by multiple users.

### Scalability

Can support many clients.

### Easier Management

Single deployment.

---

## Example

GitHub MCP Server

Capabilities:

```text
Read Repository
Create Issue
Open Pull Request
Review Code
```

---

# 9. Communication Protocols

Communication occurs using JSON-RPC.

---

# What is JSON-RPC?

JSON-RPC is a lightweight remote procedure call protocol.

Requests and responses are exchanged in JSON format.

Example:

```json
{
  "method": "read_file",
  "params": {
    "path": "notes.txt"
  }
}
```

---

# Why JSON-RPC?

### Lightweight

Minimal overhead.

### Language Independent

Works with any programming language.

### Standardized

Consistent communication.

---

# 10. STDIO Communication

Local servers commonly use STDIO.

STDIO stands for:

```text
Standard Input / Standard Output
```

---

## Flow

```text
Host
 ↓
STDIO
 ↓
Local MCP Server
```

---

## Benefits

* Fast
* Simple
* No network required

---

## Example

Filesystem MCP Server

```text
Claude Desktop
      ↓
STDIO
      ↓
Filesystem Server
```

---

# 11. HTTP + SSE Communication

Remote servers commonly use:

```text
HTTP + SSE
```

---

## HTTP

Used for sending requests.

---

## SSE

Server-Sent Events

Used for streaming responses from server to client.

---

## Flow

```text
Host
 ↓
HTTP + SSE
 ↓
Remote MCP Server
```

---

## Example

```text
Claude Desktop
      ↓
Internet
      ↓
GitHub MCP Server
```

---

# 12. Tools

Tools allow AI to perform actions.

Think:

```text
Tool = Do Something
```

---

## Examples

```text
create_file()
delete_file()
create_issue()
send_email()
```

---

## Characteristics

* Action-oriented
* Executable
* Can modify systems

---

# 13. Resources

Resources provide data.

Think:

```text
Resource = Read Something
```

---

## Examples

```text
PDF File
Database Record
Text File
Configuration File
```

---

## Characteristics

* Read-focused
* Information source
* Context provider

---

# 14. Prompts

Prompts are reusable instructions.

Think:

```text
Prompt = Reusable Template
```

---

## Examples

```text
Summarize Document

Analyze Code

Generate Report
```

---

## Characteristics

* Reusable
* Standardized
* Predefined

---

# 15. Complete Request Lifecycle

Suppose user says:

```text
Read project documentation.
```

Flow:

```text
User
 ↓
Host
 ↓
Client
 ↓
Filesystem Server
 ↓
Resource
 ↓
File Content
 ↓
Client
 ↓
Host
 ↓
User
```

---

# Memory Tricks

## Trick 1

```text
Host = Home

Client = Courier

Server = Service Provider
```

---

## Trick 2

```text
Tool = Do

Resource = Read

Prompt = Reuse
```

---

## Trick 3

```text
Local Server → STDIO

Remote Server → HTTP + SSE
```

---

# Interview Questions

## Q1. What are the three main components of MCP architecture?

* Host
* Client
* Server

---

## Q2. What is the role of the Host?

The host is the AI application that manages users, AI models, and MCP clients.

---

## Q3. What is the role of the Client?

The client communicates with MCP servers and manages requests and responses.

---

## Q4. What is the role of the Server?

The server exposes tools, resources, and prompts to AI systems.

---

## Q5. Difference between Local and Remote MCP Servers?

| Local Server | Remote Server     |
| ------------ | ----------------- |
| Same machine | Different machine |
| Uses STDIO   | Uses HTTP + SSE   |
| Faster       | Network dependent |
| More private | More scalable     |

---

## Q6. What are MCP Tools?

Tools are executable functions that perform actions.

---

## Q7. What are MCP Resources?

Resources are data sources that AI can read.

---

## Q8. What are MCP Prompts?

Prompts are reusable instruction templates provided by servers.

---

## Q9. Why is JSON-RPC used?

Because it provides a lightweight and standardized communication mechanism.

---

# Quick Revision Notes

```text
MCP Architecture

User
 ↓
Host
 ↓
Client
 ↓
Server
 ↓
Tools / Resources / Prompts

Host:
AI Application

Client:
Communication Layer

Server:
Capability Provider

Tools:
Actions

Resources:
Data

Prompts:
Templates

Local Server:
STDIO

Remote Server:
HTTP + SSE

Communication:
JSON-RPC 2.0
```

---

# Key Takeaways

* MCP architecture consists of Host, Client, and Server.
* Clients act as communication bridges.
* Servers expose capabilities through Tools, Resources, and Prompts.
* Local servers usually use STDIO.
* Remote servers usually use HTTP + SSE.
* Communication is performed using JSON-RPC.
* Understanding this architecture is essential before learning MCP Lifecycle and MCP Server Development.

---

# Next Chapter

**Chapter 04: MCP Lifecycle**

* Initialization
* Discovery
* Capability Exchange
* Request Execution
* Response Handling
* Connection Termination
