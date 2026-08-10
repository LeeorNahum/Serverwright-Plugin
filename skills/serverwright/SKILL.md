---
name: serverwright
description: Operate remote servers through Serverwright's MCP tools. Use when a task reads, changes, searches, or runs anything on a server connected to Serverwright, or when deciding which connected server to act on.
---

# Serverwright

Serverwright serves the servers in one workspace over MCP. The connection was authorized into a single workspace at sign-in, and every tool call is authorized and recorded there.

## Pick a target before acting

Call `servers` first. It lists every server this connection can operate on, each with its capabilities, its connection route and health, and its validation state. Address a server by the id it reports, never by guessing from a name.

If the server the task needs is not listed, it is not in the workspace this connection was authorized into. Say so rather than acting on a lookalike, and let the user either add the server in the Serverwright dashboard or reconnect and authorize a different workspace.

## Let capabilities shape the plan

The tool catalog is the same for every caller; whether a given server runs a verb is decided per call from the capabilities `servers` reported. Plan with those capabilities: file work belongs on any server, while console, command, power, decompress, and shell work belongs only on a server whose capabilities include that verb. A verb the target cannot run fails with a typed error naming the server, the verb, and what that server does support. Treat that as a wrong plan to revise, not a call to retry.

## Respect the approval boundary

Reads are safe to repeat. Before a destructive or hard-to-reverse action the user did not explicitly request, such as deleting files, sending power signals, or running state-changing shell commands, state what will run and on which server, and get confirmation. Every call lands in the workspace audit trail with its actor and outcome, so act as if the owner reads the trail, because they can.

## Recover from the cause, not around it

Errors name their cause: an unsupported verb, an unconfirmed host key, a connection with no usable route, a seat beyond the owner's current plan. Fix the named cause, and when the fix lives in the product, such as confirming a host key or repairing a connection, point the user at the Serverwright dashboard instead of retrying. An OAuth challenge when connecting is normal: complete the sign-in the client offers.
