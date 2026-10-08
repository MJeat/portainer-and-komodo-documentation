
Tutorial source: [here](https://www.youtube.com/watch?v=QBNaOdNSsx8)

First, install portainer ce for Linux. Here's the download link: [portainer-ce](https://docs.portainer.io/start/install-ce/server/docker/linux)




For edge compute/ edge agent, this is not for local usage. It's for testing on cloud. Use this only when you need to only have 1 central place to manage images and containers of other servers.


<img width="830" height="618" alt="image" src="https://github.com/user-attachments/assets/0796a1c8-d5a4-45d0-9625-64cd2e99afc3" />



The Stack means it runs from 1 docker compose file. 

<img width="1847" height="764" alt="image" src="https://github.com/user-attachments/assets/248586b0-9c85-49b9-ba21-26e83245ba63" /><br/>

===

For example, CTFd stack here has a lot of containers running. All of that because CTFd has one central `docker-compose.yml`  that starts all containers.

<img width="1546" height="280" alt="image" src="https://github.com/user-attachments/assets/4b62d1aa-6f5f-4ebf-a260-fb2cc1d46976" />
<img width="1355" height="606" alt="image" src="https://github.com/user-attachments/assets/8f751028-78f7-4f3f-bad0-c0926819a4a9" />


Same goes for my testing stack called `test`




# How to Connect Portainer to manage other servers' dockers

```
Your PC
├── Portainer Server
└── Docker
     └── containers

Second PC / VM
└── Docker
     └── containers
          ↑
       Portainer Agent
          ↑
          │ LAN
          ↓
     Portainer Server
```

### Phase 1 — Prepare the external Docker server

On the second machine, make sure:

- Docker is installed and running.
- You have administrator/root access.
- The machine can communicate with your Portainer machine over the LAN.

Portainer's documentation lists Docker and administrative access as prerequisites for adding a standalone Docker environment.

### Phase 2 — Add it from Portainer

On your main Portainer UI:
- Environments → Add environment → Docker Standalone → Start Wizard → Agent (Not Edge Agent)
  
<img width="288" height="234" alt="image" src="https://github.com/user-attachments/assets/25108ea1-0695-4ae0-9e21-b7df525e339b" />

<img width="1220" height="658" alt="image" src="https://github.com/user-attachments/assets/5c5e3a60-3b61-4335-828e-ba47f6b4c630" />


- Portainer will give you an Agent installation command.

Important: run that generated command on the second Docker server, not on your main PC.

Then return to Portainer and complete the environment setup.

<img width="500" height="206" alt="image" src="https://github.com/user-attachments/assets/65019c6c-1773-4d89-9ede-0ec8825d1359" />


### To access the new server
- Go to Home page and click on the right server name
<img width="904" height="529" alt="image" src="https://github.com/user-attachments/assets/f98f5665-e010-4f24-b592-1d9bae83a899" />

- You should have these ready and you are good to go with managing the containers and images

<img width="1337" height="651" alt="image" src="https://github.com/user-attachments/assets/2cf20472-199f-45f7-a866-b31400b5b176" />



# Using Edge Agent

| Feature                                                   | **Normal Agent**                                         | **Edge Agent**                                                   |
| --------------------------------------------------------- | -------------------------------------------------------- | ---------------------------------------------------------------- |
| **Connection direction**                                  | Portainer → Agent                                        | Agent → Portainer                                                |
| **Who initiates connection?**                             | Main Portainer server                                    | Remote Docker server                                             |
| **Default Agent port**                                    | `9001`                                                   | `8000` tunnel on Portainer                                       |
| **Remote server needs inbound port open?**                | **Yes**, typically `9001`                                | **Usually no**                                                   |
| **Remote server needs outbound internet/network access?** | Not necessarily                                          | **Yes**                                                          |
| **Works behind NAT?**                                     | Can be difficult                                         | **Much easier**                                                  |
| **Works behind restrictive firewall?**                    | Requires inbound access                                  | Usually easier because outbound connections are commonly allowed |
| **Security exposure**                                     | Remote Agent port must be reachable by Portainer         | Remote Agent doesn't need its API exposed publicly               |
| **Setup complexity**                                      | Simpler                                                  | Slightly more involved                                           |
| **Best for**                                              | Servers you directly control on the same/private network | Remote servers, cloud servers, NAT/firewall-restricted servers   |
| **Main Portainer must reach remote server?**              | **Yes**                                                  | **No direct inbound connection required**                        |
| **Remote server must reach main Portainer?**              | Not necessarily                                          | **Yes**                                                          |
| **Typical use**                                           | LAN / private network                                    | Internet / remote infrastructure                                 |
| **Your setup**                                            | Works                                                    | **Better choice if Server #2 is remote**                         |

- If Server #2 can comfortably accept an inbound connection from Server #1, Normal Agent is simpler.

- If Server #2 is somewhere you can't or don't want to expose an inbound Agent port, Edge Agent becomes attractive.

| Situation                                                  | Use                                  |
| ---------------------------------------------------------- | ------------------------------------ |
| Portainer + Docker are on the **same Ubuntu server**       | **Docker socket**                    |
| Two servers on your **same LAN/private network**           | **Normal Agent** is usually simplest |
| Remote VPS with a public IP                                | **Normal Agent** can work            |
| Remote server is behind NAT                                | **Edge Agent**                       |
| Remote server has a restrictive firewall                   | **Edge Agent**                       |
| You don't want to expose Agent `9001` on the remote server | **Edge Agent**                       |
| Managing many geographically distributed servers           | **Edge Agent**                       |
| Temporary lab where everything is on your LAN              | **Normal Agent**                     |
| Production infrastructure spread across networks           | **Edge Agent**                       |

## Set up Edge Agent

```
Ubuntu Server #1 — MAIN
├── Portainer
│   ├── :9443 → Web UI
│   └── :8000 → Edge Tunnel
└── CTFd :8080

              ▲
              │ Edge connection
              │
Ubuntu Server #2 — DOCKER SERVER
├── Portainer Edge Agent
└── Your Docker containers
```

#### Step 1: 

We need to change apps that use port 8000 to other ports because we want to leave the 8000 port for portainer edge agent.

#### Step 2: Create Portainer container

Portainer Edge Agent ``docker-compose.yml``:
```
services:
  portainer:
    # ============================================================
    # PORTAINER
    # ============================================================
    # Portainer is a prebuilt image, so NO Dockerfile or
    # requirements.txt is needed.
    #
    # IMPORTANT PORTS:
    #   9443 = Portainer HTTPS Web UI
    #   9000 = Portainer HTTP Web UI (optional/legacy)
    #   8000 = Portainer Edge Agent tunnel INSIDE the container
    #
    # We map HOST 8001 -> CONTAINER 8000 because CTFd uses
    # HOST 8000 on this server.
    #
    # If you ever move CTFd away from port 8000, you can change:
    #
    #   - "8001:8000"
    #
    # to:
    #
    #   - "8000:8000"
    #
    # But then make sure nothing else is using host port 8000.
    # ============================================================

    image: portainer/portainer-ce:lts

    container_name: portainer

    restart: always

    ports:
      # Portainer HTTPS Web UI
      - "9443:9443"

      # Portainer HTTP Web UI
      # You can remove this if you only use HTTPS on 9443.
      - "9000:9000"

      # EDGE AGENT TUNNEL
      #
      # HOST PORT : 8001
      #       ↓
      # CONTAINER PORT : 8000
      #
      # Edge Agents should connect to:
      # ws://YOUR_MAIN_SERVER_IP:8001
      #
      # DO NOT change.
      - "8000:8000"

    volumes:
      # Allows Portainer to manage the Docker daemon
      - /var/run/docker.sock:/var/run/docker.sock

      # IMPORTANT:
      # This stores Portainer's users, environments, settings,
      # credentials, etc.
      #
      # DO NOT delete this volume unless you intentionally want
      # to reset Portainer completely.
      - portainer_data:/data

volumes:
  # Persistent Portainer data
  portainer_data:

```

#### Step 3: Create Edge Agent

On your main Portainer UI:
- Environments → Add environment → Docker Standalone → Start Wizard → Edge Agent
- Make sure the Portainer API URL is your main server's IP. Keep the port the same.

<img width="604" height="554" alt="image" src="https://github.com/user-attachments/assets/55887429-abca-4fd8-b024-a27d863f6d1a" />

- You can also test connectivity. Copy the `docker run` command and run it on the other server. It should show like this

<img width="840" height="329" alt="image" src="https://github.com/user-attachments/assets/c1af37f2-911b-4663-b604-ccb02f5d1a8c" />


- Once success, you will see this in the Home page. Click on it and it will connect.

<img width="1318" height="167" alt="image" src="https://github.com/user-attachments/assets/c14e4f75-f2fc-457a-9b49-ba4dc15806fe" />






