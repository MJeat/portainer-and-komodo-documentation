
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








