# Chapter 04: MCP Lifecycle

> Understanding how Hosts, Clients, and Servers interact throughout the lifecycle of an MCP connection.

---

# Learning Objectives

- Understand the MCP lifecycle.
- Learn how Hosts discover MCP Servers.
- Understand capability exchange.
- Learn request-response execution.
- Understand connection management.

---

# 1. What is MCP Lifecycle?

The MCP Lifecycle is the sequence of steps that occur when a Host communicates with an MCP Server.

Think of it as:

```text
Connect
 ↓
Discover
 ↓
Execute
 ↓
Respond
 ↓
Disconnect
```

---

# 2. Complete MCP Lifecycle

```text
Host Starts
     ↓
Client Created
     ↓
Server Connected
     ↓
Initialization
     ↓
Capability Discovery
     ↓
Request Execution
     ↓
Response Handling
     ↓
Connection Closed
```

---

# 3. Phase 1: Initialization

The first step is establishing communication.

Purpose:

- Verify compatibility
- Exchange protocol information
- Prepare communication

Flow:

```text
Host
 ↓
Initialize Request
 ↓
Server
 ↓
Initialize Response
```

---

# Why Initialization?

Without initialization:

```text
Host ❌ Doesn't know:

- Server Version
- Supported Features
- Available Capabilities
```

Initialization solves this.

---

# 4. Phase 2: Capability Discovery

After initialization, the Host asks:

```text
"What can you do?"
```

The Server responds with its capabilities.

Example:

```text
Filesystem Server

Capabilities:
- Read File
- Write File
- Delete File
- Create Folder
```

---

# Discovery Flow

```text
Host
 ↓
Capability Request
 ↓
Server
 ↓
Capability List
```

---

# Why Discovery Matters?

The Host should not assume capabilities.

Example:

```text
Server A:
- Read File
- Write File

Server B:
- Read File
```

Discovery prevents invalid requests.

---

# 5. Phase 3: Tool Registration

The Server exposes available Tools.

Example:

```text
create_file()
read_file()
delete_file()
```

The Client stores this information.

---

# 6. Phase 4: Resource Registration

The Server exposes available Resources.

Examples:

```text
project.pdf
config.json
logs.txt
```

Resources provide context.

---

# 7. Phase 5: Prompt Registration

The Server exposes reusable prompts.

Examples:

```text
Analyze Code

Summarize Document

Generate Report
```

---

# 8. Phase 6: Request Execution

User sends a request.

Example:

```text
Read notes.txt
```

Flow:

```text
User
 ↓
Host
 ↓
Client
 ↓
Server
 ↓
Tool/Resource
```

---

# Example JSON-RPC Request

```json
{
  "method": "read_file",
  "params": {
    "path": "notes.txt"
  }
}
```

---

# 9. Phase 7: Response Handling

Server processes request.

Returns result.

Example:

```json
{
  "result": {
    "content": "File Content"
  }
}
```

Flow:

```text
Server
 ↓
Client
 ↓
Host
 ↓
User
```

---

# 10. Phase 8: Connection Management

The connection remains active.

Benefits:

- Faster communication
- Lower overhead
- Reuse existing session

---

# 11. Phase 9: Connection Termination

Connection closes when:

- Host shuts down
- Server shuts down
- Timeout occurs
- User disconnects

Flow:

```text
Host
 ↓
Disconnect
 ↓
Server
```

---

# Full Lifecycle Example

User:

```text
Summarize project.pdf
```

Lifecycle:

```text
Host Starts
 ↓
Server Connected
 ↓
Initialization
 ↓
Capability Discovery
 ↓
Resource Discovery
 ↓
Request Sent
 ↓
File Read
 ↓
Response Generated
 ↓
Summary Returned
```

---

# Memory Trick

```text
I D E R C

I → Initialize

D → Discover

E → Execute

R → Respond

C → Close
```

---

# Interview Questions

## Q1. What is MCP Lifecycle?

The sequence of communication stages between Host and Server.

---

## Q2. What happens during initialization?

Protocol information and compatibility details are exchanged.

---

## Q3. Why is capability discovery important?

It allows Hosts to know what Servers can do before sending requests.

---

## Q4. What is executed during request execution?

Tools, Resources, or Prompts.

---

## Q5. When is a connection terminated?

When communication is completed or the session ends.

---

# Quick Revision Notes

```text
MCP Lifecycle

1. Initialize
2. Discover
3. Register Capabilities
4. Execute Request
5. Generate Response
6. Maintain Session
7. Disconnect

Memory:

I → Initialize
D → Discover
E → Execute
R → Respond
C → Close
```

---

# Key Takeaways

- Every MCP interaction follows a lifecycle.
- Initialization establishes communication.
- Discovery reveals capabilities.
- Requests are executed through tools/resources/prompts.
- Responses are returned via JSON-RPC.
- Connections may remain active or terminate.

---

# Next Chapter

**Chapter 05: Connecting MCP Servers to Claude Desktop**
- Configuration Files
- Server Registration
- STDIO Servers
- Debugging Connections
- Real Examples
