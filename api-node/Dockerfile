# get small (alpine) node image with minimal dependencies
FROM node:19.6-alpine
# define a working directory replacing the root (/) directory by default
WORKDIR /usr/src/app
# define a environemnt variable NODE_ENB to production value
ENV NODE_ENV production 
# copie all package file into the root dir of the docker image
COPY package*.json ./
# download all dependencies (ci: clean & install) only in production 
RUN npm ci --only=production 
## option replace previous cmd: mounting cashe for downloading package
RUN --mount=type=cache,target=/usr/src/app/.npm \
	npm set cache /usr/src/app/.npm && \
	npm ci --only=production
# execute as a non root user new user name 
USER node  
# copies all the contents of the current directory/src on the host machine into the root dir of the docker image.
COPY --chown=nade:node ./src . 
# documentation for the users
EXPOSE 3000
# run this command when the container based on this image starts
CMD ["node", "index.js"]