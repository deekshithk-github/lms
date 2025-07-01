#Base Image
FROM node:20

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
RUN npx prisma generate

#We need to Build the process
#RUN --> npm run build for building the context
RUN npm run build

#Specify the Port to use
#EXPOSE --> Set Port
EXPOSE 3000

#Start Application
CMD ["npm","start"]
