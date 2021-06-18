---
title: Docker
description: Getting Started with Flagsmith on Docker
sidebar_position: 4
---

You can use docker to set up an entire [Flagsmith Feature Flag](https://www.flagsmith.com) environment locally. Just
clone the [docker repository](https://github.com/Flagsmith/flagsmith) and run docker-compose:

```bash
git clone https://github.com/Flagsmith/flagsmith.git
cd flagsmith/docker
docker-compose up
```

Wait for the images to download and run, then visit `http://localhost:8080/`. As a first step, you will need to create a
new account at [http://localhost:8080/signup](http://localhost:8080/signup)

## Architecture

The docker-compose file runs the following containers:

### Front End Dashboard - Port 8080

The Web user interface. From here you can create accounts and manage your flags. The front end is written in node.js and
React.

### REST API - Port 8000

The web user interface communicates via REST to the API that powers the application. The SDK clients also connect to
this API. The API is written in Django and the Django REST Framework.

Once you have created an account and some flags, you can then start using the API with one of the
[Flagsmith Client SDKs](https://github.com/Flagsmith?q=client&type=&language=). You will need to override the API
endpoint for each SDK to point to [http://localhost:8000/api/v1/](http://localhost:8000/api/v1/).

You can access the Django Admin console to get CRUD access to some of the core tables within the API. You will need to
create a super user account first with the following command:

```bash
# Make sure you are in the root directory of this repository
docker-compose run --rm --entrypoint "python manage.py createsuperuser" api
```

You can then access the admin dashboard at [http://localhost:8000/admin/](http://localhost:8000/admin/)

## Access Flagsmith Remotely

You will need to either open ports into your docker host or set up a reverse proxy to access the two Flagsmith services
(dashboard and API). You will also need to configure the dashboard environment variable `API_URL`, which tells the
dashboard where the REST API is located.

## Postgres Database

The REST API stores all its data within a Postgres database. Schema changes will be carried out automatically when
upgrading using Django Migrations.
