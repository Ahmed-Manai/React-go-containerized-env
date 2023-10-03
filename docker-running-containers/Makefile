### DOCKER COMPOSE COMMANDS

.PHONY: compose-build
compose-build:
	docker compose build

.PHONY: compose-up
compose-up:
	docker compose up

.PHONY: compose-up-build
compose-up-build:
	docker compose up --build

.PHONY: compose-down
compose-down:
	docker compose down

### DOCKER CLI COMMANDS

DOCKERCONTEXT_DIR:=..
DOCKERFILE_DIR:=..

.PHONY: docker-build-all
docker-build-all:
	# for the react client we using npm run dev script and using vite to serve a development copy of our front-end 
	docker build -t client-react-vite -f ${DOCKERFILE_DIR}/client-react/Dockerfile-react-vite ${DOCKERCONTEXT_DIR}/client-react/
	# copy the build version of the front-end app into an nginx based container image
	docker build -t client-react-ngnix -f ${DOCKERFILE_DIR}/client-react/Dockerfile-react-ngnix ${DOCKERCONTEXT_DIR}/client-react/
	docker build -t api-node -f ${DOCKERFILE_DIR}/api-node/Dockerfile-api-node ${DOCKERCONTEXT_DIR}/api-node/
	docker build -t api-golang -f ${DOCKERFILE_DIR}/api-golang/Dockerfile-api-golang ${DOCKERCONTEXT_DIR}/api-golang/


# to setup this on linux use : export DATABASE_URL=postgres://postgres:ahmed@db:5432/postgres
DATABASE_URL:=postgres://postgres:ahmed@db:5432/postgres


.PHONY: docker-run-all
docker-run-all:
	echo "$$DOCKER_COMPOSE_NOTE"

	# Stop and remove all running containers to avoid name conflicts
	$(MAKE) docker-stop

	$(MAKE) docker-rm

	# Create a docker network (default as using the bridge driver network)
	docker network create my-network

	# postgres Container connect the my-network and restart if stopped
	docker run -d \
		--name db \
		--network my-network \
		-e POSTGRES_dockerSSWORD=ahmed \
		-v pgdata:/var/lib/postgresql/data \
		-p 5432:5432 \
		--restart unless-stopped \
		postgres:15.1-alpine

	# node api container make sure to export DATABASE_URL
	docker run -d \
		--name api-node \
		--network my-network \
		-e DATABASE_URL=${DATABASE_URL} \
		-p 3000:3000 \
		--restart unless-stopped \
		--link=db \
		api-node

	# golang api
	docker run -d \
		--name api-golang \
		--network my-network \
		-e DATABASE_URL=${DATABASE_URL} \
		-p 8080:8080 \
		--restart unless-stopped \
		--link=db \
		api-golang

	# run react vite 
	docker run -d \
		--name client-react-vite \
		--network my-network \
		-v ${PWD}/vite.config.js:/usr/src/app/vite.config.js \
		-p 5173:5173 \
		--restart unless-stopped \
		--link=api-node \
		--link=api-golang \
		client-react-vite

	# react nginx
	docker run -d \
		--name client-react-nginx \
		--network my-network \
		-p 80:8080 \
		--restart unless-stopped \
		--link=api-node \
		--link=api-golang \
		client-react-ngnix 


.PHONY: docker-stop
docker-stop:
	-docker stop db
	-docker stop api-node
	-docker stop api-golang
	-docker stop client-react-vite
	-docker stop client-react-nginx

.PHONY: docker-rm
docker-rm:
	-docker container rm db
	-docker container rm api-node
	-docker container rm api-golang
	-docker container rm client-react-vite
	-docker container rm client-react-nginx
	-docker network rm my-network

define DOCKER_COMPOSE_NOTE

🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨

❯ NOTE:

This command runs the example app with a bunch
of individual docker run commands. This is much
easier to manage with docker-compose (see 
docker-compose.yml and compose make targets above)

🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨🚨


endef
export DOCKER_COMPOSE_NOTE