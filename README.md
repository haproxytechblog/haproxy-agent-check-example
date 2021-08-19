# HAProxy Agent Check Example

This is a small program that demonstrates an agent health check for HAProxy. Written in Go, it 
checks the (Linux) system's CPU idle time. If that metric falls below 10, the string 50%\n is sent back 
to HAProxy, which sets the server's weight in the load balancer to half of what it was.

## How to use it

1. Build the agent program with Go:

   ```bash
   GOOS=linux GOARCH=amd64 go build -v -o ./agent .
   ```

1. Copy the `agent` program to your webserver and run it:

   ```bash
   ./agent
   ```


## Demo

1. Build and run the demo using Docker Compose:

   ```bash
   cd demo
   sudo docker-compose build
   sudo docker-compose up
   ```

   This hosts a website at http://localhost. You can see the HAProxy Stats page at http://localhost:8404.

1. To artifically increase CPU usage on the webserver, run the `stress-ng` command:

   ```
   sudo docker-compose exec webserver1 stress-ng --cpu 8 --timeout 20
   ```

1. Watch the HAProxy Stats page at http://localhost:8404/ to see the server's weight change.

   You can also change the action that HAProxy takes. For example, change the **agent.go** code
   so that it sets the server to *up* or *down*:

   ```go
   c.Send("up\n")

   ...

   c.Send("down\n")
   ```