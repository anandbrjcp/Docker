# specify a base image 

#FROM alpine
FROM node:alpine
WORKDIR /usr/app

#install dependencies 

#COPY ./ ./
COPY ./package.json ./
RUN npm install
COPY ./ ./


# Default command

CMD ["npm", "start"]
