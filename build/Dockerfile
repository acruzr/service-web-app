### STAGE 1: Build ###
# AS build introduce a multi-stage Docker build which will make the container smaller and the work more automated
# it’s main goal was to create smaller containers without loosing the readability of a Dockerfile. With this approach we can
# divide building a Docker image into smaller phases (stages) where result of previous one (or part of it) can be used in
# another.
# To put it into our context, we’ll divide our Docker build into two stages:
# 1. compiling the source code into prod, dev, qa ready output
# 2. running compiled app in a Docker image
# Only compiled output from first stage will be moved to the second so small size of the container will be preserved.
FROM node AS build
WORKDIR /app
ADD package.json .
RUN npm install
COPY . .
RUN npm run build
#CMD npm run start -- --port 4500 --host 0.0.0.0
### STAGE 2: Run ###
FROM nginx
#COPY docker/nginx/default.conf /etc/nginx/conf.d/default.conf  ## default hostname is localhost and port 80
# 'from' flag tell Docker that it needs to copy compiled files from build stage ###
COPY --from=build /app/dist/service-web-app /usr/share/nginx/html
EXPOSE 80   ## as the default port that nginx expose is 80 we expose that one
# run nginx
CMD ["nginx", "-g", "daemon off;"]
