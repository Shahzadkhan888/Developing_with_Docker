## Demo App — Developing with Docker & AWS ECR

This is a simple demo application that demonstrates how to containerize a Node.js app with MongoDB using Docker and push it to a private AWS Elastic Container Registry (ECR).

---

- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage
---

## Run Locally with Docker

**Step 1:** Create a Docker network
```bash
docker network create mongo-network
```

**Step 2:** Start MongoDB
```bash
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo
```

**Step 3:** Start Mongo Express
```bash
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password -e ME_CONFIG_MONGODB_SERVER=mongodb --net mongo-network --name mongo-express mongo-express
```

**Step 4:** Access Mongo Express in your browser
```
http://localhost:8081
```

Create:
- Database: `user-account`
- Collection: `users`

**Step 5:** Start the Node.js application
```bash
cd app
npm install
node server.js
```

**Step 6:** Access the application in your browser
```
http://localhost:3000
```

---

## Run with Docker Compose

**Step 1:** Start services
```bash
docker-compose -f docker-compose.yaml up
```

Access Mongo Express at:
```
http://localhost:8080
```

Create a database `my-db` and collection `users`.

**Step 2:** Start Node.js app manually
```bash
cd app
npm install
node server.js
```

Access the app:
```
http://localhost:3000
```

---

## Build a Docker Image

To build the app image:
```bash
docker build -t my-app:1.0 .
```

---

## Push Image to AWS ECR

**Step 1:** Login to ECR
```bash
aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 093646564182.dkr.ecr.eu-central-1.amazonaws.com
```

**Step 2:** Tag the image
```bash
docker tag my-app:1.0 093646564182.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

**Step 3:** Push the image
```bash
docker push 093646564182.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

Your image will now appear in your **AWS ECR repository** and can be deployed to ECS, EC2, or Fargate.

---

## Deployment Details
- Repository: `my-app`
- Tag: `1.0`
- Region: `eu-central-1`
- Deployment: AWS EC2 (using pulled image from ECR)

---

## Screenshots (Optional to Add)
1. AWS ECR repository showing the pushed image
2. Terminal output showing successful push
3. App running in the browser

---

## What I Learned
- Building and tagging Docker images
- Managing multi-container setups with Docker networks
- Pushing private images to AWS ECR
- Basic app deployment workflow from local to cloud
