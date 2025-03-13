<<<<<<< HEAD <<<<<<<

# Flask App with MySQL Docker Setup
Docker integration with Flask and Mysql

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites

- Docker
- Git (optional, for cloning the repository)

## Setup

1. Pull form Docker Hub or Clone this repository:

   ```bash
   docker pull ashwin369/flask-backend-app:latest
   ```

2. Download docker-compose.yml file from this repository:

   ```bash
   wget https://github.com/ashwin12377/flask-backend-app/blob/main/docker-compose.yml
   ```

## Usage

1. Start the containers using Docker Compose:

   ```bash
   docker compose up -d
   ```

2. Access the Flask app in your web browser:

   - Frontend: http://localhost
   - Backend: http://localhost:5000

4. Interact with the app:

   - Visit http://localhost to see the frontend. You can submit new messages using the form.
   - Visit http://localhost:5000/insert_sql to insert a message directly into the `messages` table via an SQL query.

## Cleaning Up

To stop and remove the Docker containers use the following command:

```bash
docker-compose down
```

## To run this application using  without docker-compose (manually)

- First create a docker image from Dockerfile
```bash
docker build -t flaskapp .
```

- Now, make sure that you have created a network using following command
```bash
docker network create  -d bridge flaskapp-ntwk
```

- Attach both the containers in the same network, so that they can communicate with each other

i) MySQL container 
```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7

```
ii) Backend container
```bash
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest

```
3. Create the `messages` table in your MySQL database ( if table not exist ):
- Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL commands:
```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
     ```   
     

