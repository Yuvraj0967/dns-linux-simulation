# DNS Linux Simulation

A small Python UDP simulation of DNS resolution for a Linux development environment. It demonstrates how a recursive resolver forwards a hostname query to an authoritative server and returns the resulting record to a test client.

## How it works

The authoritative server listens on UDP port 5354 and stores a few project-specific records in an in-memory dictionary. The recursive resolver listens on UDP port 5300, forwards each hostname to the authoritative server, and sends the response back to the client. The test client sends a hostname such as www.cisco.test to the resolver and prints the returned record. The ports are above 1024 so the simulation can run without sudo.

## Components

| Component | Language | Role |
| --- | --- | --- |
| The Authoritative Server | Python | UDP server on port 5354 with A/CNAME records |
| The Recursive Resolver | Python | UDP forwarding resolver on port 5300 |
| The Test Client | Python | Sends a hostname query and prints the response |

## Run it

Open three Linux terminals in the repository directory and run the components in this order:

~~~bash
python3 "The Authoritative Server"
python3 "The Recursive Resolver"
python3 "The Test Client"
~~~

The first two commands keep their servers running. Run the test client after both servers report that they are online.

## Example output

~~~text
Requesting IP for: www.cisco.test
Response: {'type': 'A', 'value': '192.168.1.10', 'ttl': 300}
~~~

The server terminals also log the received query as it passes through the resolver and authoritative server.

## What I learned / limitations

- This is a teaching simulation over localhost UDP, not a standards-compliant DNS implementation.
- Records are stored in memory and the servers do not implement DNS packet encoding, caching, retries, or concurrency controls.
- The resolver and authoritative server must be started manually in separate terminals.
