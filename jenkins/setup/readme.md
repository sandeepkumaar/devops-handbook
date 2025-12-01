## Setup Jenkins with Docker
- create a network for docker daemon and Jenkins
- Run Docker Daemon is used for Jenksin to spin up ephemeral containers when agent:docker
- Run Jenkins+Docker CLI + Plugins image and connect to daemon

## Docker Compose (Recommended)

```bash
# Start both Jenkins and Docker daemon
docker-compose up -d

# View logs
docker-compose logs -f jenkins

# Stop everything
docker-compose down

# Stop and remove volumes (clean slate)
docker-compose down -v
```

## Docker run
### Create network
```bash
docker network create jenkins
```

### Install Docker Daemon
```bash
docker run \
--name jenkins-docker \
--rm \
--detach \
--privileged \
--network jenkins \
--network-alias docker \
--env DOCKER_TLS_CERTDIR=/certs \
--volume jenkins-docker-certs:/certs/client \
--volume jenkins-data:/var/jenkins_home \
--publish 2376:2376 \
docker:dind \
--storage-driver overlay2
```
## Install Jenkins and connect to docker daemon
Create a customised docker image with Jenkins + Docker plugin
and run 
```bash
docker run \
--name jenkins-blueocean \
--restart=on-failure \
--detach \
--network jenkins \
--env DOCKER_HOST=tcp://docker:2376 \
--env DOCKER_CERT_PATH=/certs/client \
--env DOCKER_TLS_VERIFY=1 \
--publish 8081:8080 \
--publish 50000:50000 \
--volume jenkins-data:/var/jenkins_home \
--volume jenkins-docker-certs:/certs/client:ro \
myjenkins-blueocean:2.528.2-1
```

## Access jenkins portal
```bash
cat /var/jenkins_home/secrets/initialAdminPassword 
```

## Jenkins + Docker + Host 
```
%%{init: {'theme':'base', 'themeVariables': { 'primaryColor':'#fff4e6','primaryTextColor':'#1a1a1a','primaryBorderColor':'#000','lineColor':'#000','secondaryColor':'#e3f2fd','tertiaryColor':'#f3e5f5','noteBkgColor':'#fff9c4','noteTextColor':'#1a1a1a','noteBorderColor':'#000','fontSize':'14px'},'flowchart':{'useMaxWidth':false,'htmlLabels':true,'curve':'basis','padding':20,'nodeSpacing':50,'rankSpacing':50}}}%%

graph TB
    subgraph HOST["🖥️ HOST MACHINE (macOS)<br/>Provides: CPU, RAM, Disk, Network<br/>Runs: Docker Engine"]
        subgraph NETWORK["🌐 DOCKER NETWORK (jenkins-net)"]
            JENKINS["📦 JENKINS MASTER<br/>(your Dockerfile)<br/>━━━━━━━━━━━━━━<br/>• Jenkins UI (8080)<br/>• Orchestrates jobs<br/>• Has docker-ce-cli<br/>• Stores job configs"]
            
            DIND["🐳 docker:dind<br/>(Docker Daemon)<br/>━━━━━━━━━━━━━━<br/>• Exposes Docker API<br/>  (port 2376/2375)<br/>• Builds/runs containers<br/>• Manages images"]
            
            subgraph AGENTS["⚡ EPHEMERAL AGENT CONTAINERS<br/>(created on-demand, destroyed after completion)"]
                NODE["📦 node:lts<br/>━━━━━━━━━━<br/>• git clone<br/>• npm install<br/>• npm test<br/>• npm build"]
                
                MAVEN["📦 maven:3.9<br/>━━━━━━━━━━<br/>• mvn build<br/>• mvn test"]
                
                PYTHON["📦 python:3<br/>━━━━━━━━━━<br/>• pytest<br/>• pip"]
            end
        end
        
        subgraph VOLUMES["💾 VOLUMES (persistent storage)"]
            VOL1["jenkins_home → /var/jenkins_home<br/>(configs, jobs, logs)"]
            VOL2["docker-certs → TLS certs<br/>(secure Docker API access)"]
        end
    end
    
    subgraph EXTERNAL["🌍 EXTERNAL NETWORK"]
        GIT["GitHub/GitLab"]
        NPM["npm registry"]
        DOCKER_HUB["Docker Hub"]
    end
    
    JENKINS <-->|"Docker API calls"| DIND
    JENKINS -.->|"Creates agents via<br/>Docker API"| AGENTS
    DIND -.->|"Spawns containers"| AGENTS
    
    AGENTS -->|"Fetch code"| GIT
    AGENTS -->|"Download packages"| NPM
    DIND -->|"Pull images"| DOCKER_HUB
    
    classDef hostStyle fill:#e1f5ff,stroke:#0288d1,stroke-width:3px
    classDef networkStyle fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef containerStyle fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    classDef agentStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef volumeStyle fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    classDef externalStyle fill:#eceff1,stroke:#455a64,stroke-width:2px
    
    class HOST hostStyle
    class NETWORK networkStyle
    class JENKINS,DIND containerStyle
    class AGENTS,NODE,MAVEN,PYTHON agentStyle
    class VOLUMES,VOL1,VOL2 volumeStyle
    class EXTERNAL,GIT,NPM,DOCKER_HUB externalStyle
```
