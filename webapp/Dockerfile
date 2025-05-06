#Base Image
#AS is referring to COPY in deployment
FROM node:16 AS builder

#Prepare Seperate Directory for Frontend
#RUN --> Run Commands in image
RUN mkdir /frontend 

#Copy is used copy the files into directory
COPY . /frontend

#Set Frontend as Project Path
#WORKDIR --> Used to set path
WORKDIR /frontend

#Start build Process
RUN npm install

#We need to Build the process
#RUN --> npm run build for building the context
RUN npm run build

#Base Image for Deployment
FROM nginx

#Copy Code
#COPY SRC DEST
COPY --from=builder /frontend/dist /usr/share/nginx/html
