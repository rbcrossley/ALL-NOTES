# Differences between active open and passive open?

Active open
- client process using TCP takes the "active role"
- initiates the connection by actually sending a TCP message to start the connection(SYN message)
Passive open
- it's called passive open because aside from indicating that the process is listening, the server process does nothing.
- server initiates this
- It says to client "I am here, and I am waiting for clients that may wish to talk to me to send me a message on the following port number"
Generally we consider server as passive participant and client as active participant.
# SYN segment although carrying no data; consumes a sequence number. Design implications?

