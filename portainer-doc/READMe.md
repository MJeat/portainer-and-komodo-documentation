
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

Phase 1 — Prepare the external Docker server

On the second machine, make sure:

- Docker is installed and running.
- You have administrator/root access.
- The machine can communicate with your Portainer machine over the LAN.

Portainer's documentation lists Docker and administrative access as prerequisites for adding a standalone Docker environment.

Phase 2 — Add it from Portainer

On your main Portainer UI:
- Environments → Add environment → Docker Standalone → Start Wizard → Agent (Not Edge Agent)
  
<img width="288" height="234" alt="image" src="https://github.com/user-attachments/assets/25108ea1-0695-4ae0-9e21-b7df525e339b" />

<img width="1220" height="658" alt="image" src="https://github.com/user-attachments/assets/5c5e3a60-3b61-4335-828e-ba47f6b4c630" />


- Portainer will give you an Agent installation command.

Important: run that generated command on the second Docker server, not on your main PC.

Then return to Portainer and complete the environment setup.

<img width="500" height="206" alt="image" src="https://github.com/user-attachments/assets/65019c6c-1773-4d89-9ede-0ec8825d1359" />









