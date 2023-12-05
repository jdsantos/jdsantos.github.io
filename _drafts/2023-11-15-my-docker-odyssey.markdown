---
title: "My Docker Odyssey - a developer's tale"
layout: post
date: 2023-11-15 23:00
image: /assets/images/posts/20231115/1.png
headerImage: true
tag:
- docker
- containers
star: false
category: blog
author: jdsantos
description: How i got into Docker
---

# Embracing Docker: A Developer's Odyssey

As a seasoned developer, I've witnessed the evolution of software deployment strategies over the years. In the early 2010s, my team and I relied on Debian installation packages for distributing and updating our software. It worked, but it wasn't without its challenges.

Enter Docker – a game-changer that reshaped the way we approached software development and deployment.

## The Docker Beginning

Our first encounter with Docker was an experiment, an attempt to containerize a small component of our system. To our delight, it not only worked seamlessly but also showcased remarkable improvements in terms of consistency and reproducibility.

```bash

# Running a Docker container
docker run -d -p 8080:80 my-web-app

```

```yaml

# Docker Compose file example
version: '3'
services:
  web-app:
    image: my-web-app
    ports:
      - "8080:80"
    depends_on:
      - database
  database:
    image: postgres:latest
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
```

Dockerizing Everything
One of the most transformative aspects was the ease of handling diverse technologies within a unified containerized environment. Docker provided a standardized way to package, ship, and run applications, irrespective of their underlying technologies.

For relational databases like PostgreSQL, crafting images with custom scripts became a streamlined process. Node.js microservices danced effortlessly within containers, and even complex Laravel PHP apps found their home in this modular, scalable ecosystem.

bash
Copy code
# Pushing Docker image to Azure Container Registry
docker tag my-web-app myregistry.azurecr.io/my-web-app:v1
docker push myregistry.azurecr.io/my-web-app:v1
A Shift in Mindset
Fast forward to the present day, and Docker is no longer an afterthought but the very foundation of our development process. Before the first line of code is written, we're already sketching out production-ready Dockerfiles with a focus on minimal footprint.

The paradigm shift is evident. Docker isn't just a tool; it's a mindset that influences how we conceive and execute web applications. Whether it's for development purposes or preparing for a production launch, the dockerized approach has become intrinsic to our workflow.

Azure Cloud Private Registry
As we expanded our containerized approach, we found a reliable ally in Azure Cloud Private Registry. Distributing our software seamlessly became a reality with a secure and scalable registry.

bash
Copy code
# Authenticating Docker to Azure Container Registry
az acr login --name myregistry
Conclusion
My journey with Docker is not just a story of adopting a technology; it's a testament to the transformative power of containerization. Docker has not only simplified our deployment processes but has fundamentally altered the way we think about architecting and deploying web applications.

In a world where adaptability and efficiency are paramount, Docker has become the cornerstone of our development journey. As we continue to explore new horizons, one thing is certain – Docker is not just a tool in our toolbox; it's an integral part of our developer DNA.
