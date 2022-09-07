## Description

Solution presented to EMIL for the challenge.

## Set up the environment

### The Environment

![image](https://user-images.githubusercontent.com/25822915/188864066-5f5651f5-c69f-413b-87df-91d34a3cb2d5.png)

When executing the docker-compose.yaml file:
 - Downloads the mongo image from docker. The official image of mongoDB database. The port 28000 is opened in case you want to access it.
 - Downloads the mongo-express image. The official image of mongo-express. This application connects to the mongo database and helps you to see and modify the content. You can access it at http://localhost:9000.
 - Builds the api-gateway image. This application manages the user authentication and send the information about coordinates to grpc-microservice using GRPC as a Transport.
 - Builds the grpc-microservice, a microservice that receives coordinates and save them in the mongoDB.
 - Under the hood docker creates a new network and all the containers are able to know each other by the name.


### Prerequisites
 - You have to be installed git.
 - You have to be installed Docker and support the linux subsystem (all the images are linux-based).
 - You have to be installed Postman or similar.

### Setting up
To set up the environment you have to:
 - Clone this repo
 - Initialize and update the submodules
 - Execute the docker compose

The two first steps are covered by this command:
```bash
git clone --recurse-submodules https://github.com/ricomasgu/emil-challenge.git
```
Change to emil-challenge directory:
```bash
cd emil-challenge
```
Build the whole environment:
```bash
docker compose -f docker-compose.yaml up
```
Now you are ready to check the app.

## Checking the app

### Create a user
I did not have time to create a seeds file, then you have to create a user manually with mongo-express.

Go to http://localhost:9000 -> test -> users and create a new document.

<img src="https://user-images.githubusercontent.com/25822915/188863740-dd611b95-7f49-43a8-babf-ec141e600335.png" with="400" height="400">

### Log in the application
You can log in the application with postman or a similar application.
What you have to do is execute a POST localhost:6000/auth/login request including in the body a raw JSON with the username and the password you used in the last step, for example:
```json
{
  "username": "rick",
  "password": "Pa12345678"
}
```
You will receive a JSON with all the information as a response if everything goes well.

Then you can introduce new coordinates. If you try to access without authentication, you will see a 403.

We can introduce new coordinates with a POST localhost:6000/coordinates/new-coordinates request including in the body a raw JSON with the latitude and the longitude of the coordinates you are, for example:
```json
{
  "latitude": "41.3874",
  "longitude": "2.1686"
}
```
You will receive a message "Coordinates are saved!" if everything goes well.

**I suggest you to have both prepared in advance because I put a minute as a timeout in the session.**

### Setting down
To setting down the application.

```bash
docker compose -f docker-compose.yaml down
```

## Test

There are no tests configured.

## Next steps
There are a lot of improvements:
 - I did not use the TDD methodology. The idea is create the tests before you implement the functions.
 - The first thing coming up to my head on future features, is to support the vehicle and mobile authentication.
 - Logging
 - Error handling
 - Seed file to include at least one user to play.
 - Right now, the coordinates saved are not linked to the user, I need to fix that.

## Considerations
 - It was my first time with Nest, Docker and Microservices and it took time to learn how to use them.
 - If you want to know the path I followed (only the videos that I think are cool):
   - [Nest Overview](https://www.youtube.com/watch?v=0M8AYU_hPas)
   - [Nest API](https://www.youtube.com/watch?v=F_oOtaxb0L8&t)
   - [Nest MongoDB](https://www.youtube.com/watch?v=ulfU5vY6I78)
   - [Nest Authentication](https://www.youtube.com/watch?v=_L225zpUK0M&t)
   - [Nest Microservices 1](https://www.youtube.com/watch?v=w7zJNMOIRbw)
   - [Nest Microservices 2](https://www.youtube.com/watch?v=IpoaVi9iPWI)
   - [Nest Microservices 3](https://www.youtube.com/watch?v=CggqI_82ICc)
   - [Nest Microservices 4](https://www.youtube.com/watch?v=OuyxRE9xLw4)
   - [Docker](https://www.youtube.com/watch?v=3c-iBn73dDE)
 - You will see that I used the samples I watched to develop this application.

**I suggest to you see the 4 parts of microservices in order to follow better.**
