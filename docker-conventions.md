## Docker Conventions
## Docker Resource mapping \<External\>:\<Internal\>
- External resource (host, network, or Docker-managed)
- Internal container resource

### Examples
```yaml
# Port mapping: External host port : Internal container port
docker run -p 8080:80 nginx

# Host path volume: External host path : Internal container path  
docker run -v /host/data:/container/data nginx

# Docker Compose examples
services:
  app:
    ports:
      - "8080:80"        # Port: External host : Internal container
    
    volumes:
      - /host/data:/app/data              # Host path: External : Internal
      - myvolume:/app/cache               # Named volume: External : Internal
      - certs:/certs:ro                   # Read-only: External : Internal : Options

volumes:
  myvolume:     # Named volume declaration
  certs:        # Another named volume
```
