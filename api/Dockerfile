#Base Image
FROM node:16

#Prepare Seperate Directory for backend
#RUN --> Run Commands in image
RUN mkdir /backend 

#Copy is used copy the files into directory
COPY . /backend

#Set Backend as Project Path
#WORKDIR --> Used to set path
WORKDIR /backend

#Start build Process
RUN npm install
RUN npx prisma generate && npx prisma db push

#We need to Build the process
#RUN --> npm run build for building the context
RUN npm run build

#Specify the Port to use
#EXPOSE --> Set Port
EXPOSE 8080

#Start Application 
CMD ["node","build/index.js"] 
