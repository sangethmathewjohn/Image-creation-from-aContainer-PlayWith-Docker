### Image creation from a container

Let’s start by running an interactive shell in a ubuntu container:

      docker container run -it ubuntu bash

As you know from earlier labs, you just grabbed the image called “ubuntu” from Docker Store and are now running the bash shell inside that container.1

To customize things a little bit we will install a package called figlet in this container. 
Your container should still be running so type the following commands at your ubuntu container command line:

      apt-get update
      apt-get install -y figlet
      figlet "hello docker"

You should see the words “hello docker” printed out in large ascii characters on the screen. 
Go ahead and exit from this container

      exit

To start, we need to get the ID of this container using the ls command (do not forget the -a option as the non running container are not returned by the ls command).

      docker container ls -a

Try typing the command:

      docker container diff <container ID> 

for the container you just created.
You should see a list of all the files that were added to or changed in the container when you installed figlet.
Docker keeps track of all of this information for us. This is part of the layer concept we will explore in a few minutes.

Now, to create an image we need to “commit” this container. Commit creates an image locally on the system running the Docker engine.
Run the following command, using the container ID you retrieved, in order to commit the container and create an image out of it.

      docker container commit CONTAINER_ID

      docker image ls

You should see something like this:

        REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
        <none>              <none>              a104f9ae9c37        46 seconds ago      160MB
        ubuntu              latest              14f60031763d        4 days ago          120MB

Adding this information to an image is known as tagging an image. From the previous command, get the ID of the newly created image and tag it so it’s named ourfiglet:

        docker image tag <IMAGE_ID> ourfiglet
        docker image ls

Now we have the more friendly name “ourfiglet” that we can use to identify our image.

        REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
        ourfiglet           latest              a104f9ae9c37        5 minutes ago       160MB
        ubuntu              latest              14f60031763d        4 days ago          120MB
    
### Image creation using a Dockerfile

Type the following content into a file named index.js. You can use vi, vim or several other Linux editors in this exercise. If you need assistance with the Linux editor commands to do this follow this footnote2.

      var os = require("os");
      var hostname = os.hostname();
      console.log("hello from " + hostname);
    
Create a file named Dockerfile and copy the following content into it. Again, help creating this file with Linux editors is here 3.

      FROM alpine
      RUN apk update && apk add nodejs
      COPY . /app
      WORKDIR /app
      CMD ["node","index.js"]
   
Let’s build our first image out of this Dockerfile and name it hello:v0.1:

      docker image build -t hello:v0.1 .
    
We then start a container to check that our applications runs correctly:

      docker container run hello:v0.1

You should then have an output similar to the following one (the ID will be different though).

      hello from 92d79b6de29f





