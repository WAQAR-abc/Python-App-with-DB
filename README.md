 
# Simple PYTHON App with MySQL DB (Dockerfile + Docker-Compose)

This is a simple Python Todo List App that interacts with a MySQL database. The app allows users to submit todo items, which are then stored in the database and displayed on the frontend.

## Prerequisites

Before you begin, make sure you have the following installed:

- Docker
- Git (optional, for cloning the repository)

## Setup

1. Clone this repository (if you haven't already):
   ```bash
   git clone https://github.com/WAQAR-abc/Python-App-with-DB.git
   ```

2. Navigate to the project directory:
   ```bash
   cd Python-App-with-DB
   ```
   
## Run the App using Docker-Compose

1. Start the containers using Docker Compose:
   ```bash
   docker-compose up --build
   ```
   If you want to run in background:
   ```bash
   docker-compose up --build -d
   ```

2. Access the Flask app in your web browser:

   - Todo List App: http://localhost:5000  or  http://your-local-ip:5000

## Cleaning Up

To stop and remove the Docker containers, press `Ctrl+C` in the terminal where the containers are running, or use the following command:
```bash
docker-compose down
```
- If you want to delete the data completely, then do the following:
Note: this will not delete volume by itself (it meant to be ignored by compose down so that data get's persistent), you have to delete volumes manually:
```bash
docker volume rm todo-net
```
Also you'll see compose have also created the folder "mysql-data" to store data, if you want to delete database data then delete this folder also:
```bash
rm -rf mysql-data
```

## Run the App without using Docker-Compose

- First create a docker image from Dockerfile
```bash
docker build -t todo-app .
```

- Now, make sure that you have created a network using following command
```bash
docker network create todo-net
```

- Attach both the containers in the same network, so that they can communicate with each other

i) MySQL DB container:
```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=todo-net \
    -e MYSQL_DATABASE=todo_app_db \
    -e MYSQL_ROOT_PASSWORD=root \
    -p 3306:3306 \
    mysql:8.0.27

```
ii) Todo App container:
```bash
docker run -d \
    --name todo-app \
    --network=todo-net \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=root \
    -e MYSQL_DB=todo_app_db \
    -p 5000:5000 \
    todo-app:latest

```

## Notes

- This is a basic setup for demonstration purposes. In a production environment, you should follow best practices for security and performance.

- If you encounter issues, check Docker logs and error messages for troubleshooting.

```
