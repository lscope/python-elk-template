# python-ELK-template
A template repository to track python logs into ELK stack.

## Prerequisites

In order to use this repository, you should be familiar with base concepts of these topics:

- Python
- ELK
- Docker

## Repo structure

- `.devcontainer` → folder for devcontainer.json configuration file. Here you can find basic configurations for you VSCode devcontainer environment. If you want to use it, make sure your Docker Desktop is running, type `Ctrl+Shift+P` inside VSCode window and select for `Dev Containers: Rebuild and Reopen in Container`. VSCode will automatically open the same view of your project, but inside e a container.
- `app` → here the python code whose logs you want to track.
- `cli` → .sh scripts to initialize your dev environment. If you are working in a devcontainer, these scripts will be run automatically.
- `logstash` → this folder contains the `logstash.conf` configuration file. Please check the official documentation to dive deeper.
- `compose.yaml` → docker-compose file to start ELK and Python services.
- `Dockerfile` → to build your app python service.

## How to use it

### Local

Inside your terminal window, move under your project path and run the following commands:

```bash
pip install -r requirements.txt
docker compose up -d --build
```

### DevContainer

If you activated devcontainer all should be set up. At the start of your devcontainer, docker automatically runs the cli/start.sh file, which starts all compose services.

Just keep in mind that now you are running docker containers inside a docker container, so you won’t be able to see those in your docker desktop. If you want to check the status of python services you must do it through command line, with commands like:

```bash
docker ps --all
docker logs <CONTAINER_ID>
```

### Check logs

<aside>
⚠️ This is the same for both local and devcontainer development

</aside>

Now that all compose services are up and running, next step is to check your python logs inside the Kibana dashboard.

Inside your browser, go to **`http://localhost:5601`** IP address. It could take some minutes and you could probably have to refresh the page some times, but after all you will be redirected to this home page (if you see the message ***“Kibana server is not ready yet”***, just wait and refresh the page as many times as needed):

![Untitled](images/kibana%20home.png)

Once you are here, follow these steps:

1. On the left burger menu, go under “Management” section and click on `Stack Management`
2. On the left side of the page (without opening the sidebar menu), under “Kibana” section, click on `Index Patterns`
3. Click on the button `create index pattern`
4. The index pattern you need to create is “logstash-*”, then click `Next step`. Here you may encounter an error telling you that your index pattern doesn’t match any source. It just need few minutes to fix itself and find some logs. Just try to refresh the page few minutes after and it should go as expected.
5. Select your timestamp column (you can go with the suggested one)
6. Now you are ready to look for your logs. Open the sidebar menu, and click on `discover` under “Kibana” section.
7. Here you can filter your logs based on indexes or time, but even without filters, you should be able to see them under the `message` label.
    
    ![Untitled](images/kibana%20logs.png)
    

## Implement your own python app

As for now you just worked with a basic example of python app, but you are encouraged to implement your own. Just put your code inside `./app` folder.
