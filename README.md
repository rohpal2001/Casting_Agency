# Casting Agency API

This project is the final project for my Udacity FullStack Developer Nanodegree.

The Casting Agency models a company that is responsible for creating movies and managing and assigning actors to those movies. You are an Admin within the company and are creating a system to simplify and streamline your process.

This Application does not have a frontend implemented. It is a server only application at the moment. 

## Project Motivation
The Casting Agency Project models a company that is responsible for creating movies and managing and assigning actors to those movies. You are an Admin within the company and are creating a system to simplify and streamline your process. 


This project is simply a workspace for practicing and showcasing different set of skills related with web development. These include data modelling, API design, authentication and authorization and cloud deployment.

## Capstone Project for Udacity's Full Stack Developer Nanodegree
Heroku Link: https://paro-capstone.herokuapp.com

While running locally: http://localhost:5000

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python).

#### Virtual Enviornment

Recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/).

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py. 

## Running the server

Before running the application locally, make the following changes in the `app.py` file in root directory:
- Replace the following import statements
  ```
    from database.models import db_drop_and_create_all, setup_db, Actor, Movie
    from auth.auth import AuthError, requires_auth
  ```
  with
  ```
    from .database.models import db_drop_and_create_all, setup_db, Actor, Movie
    from .auth.auth import AuthError, requires_auth
  ```
- Also, uncomment the line `db_drop_and_create_all()` on the initial run to setup the required tables in the database.

To run the server, execute:

```bash
export DATABASE_URL=<database-connection-url>
export FLASK_APP=app.py
flask run --reload
```

Setting the `FLASK_APP` variable to `app.py` directs flask to use the `app.py` file to find the application. 

Using the `--reload` flag will detect file changes and restart the server automatically.

## API Reference

## Getting Started
Base URL: This application can be run locally. The hosted version is at `https://paro-capstone.herokuapp.com`.

Authentication: This application requires authentication to perform various actions. All the endpoints require
various permissions, except the root (or health) endpoint, that are passed via the `Bearer` token.

The application has three different types of roles:
- User
  - can only view the list of artist and movies and can view complete information for any actor or movie
  - has `get:actors, get:actors-info, get:movies, get:movies-info` permissions
- Manager
  - can perform all the actions that `User` can
  - can also create an actor and movie and also update respective information
  - has `patch:actor, patch:movie, post:actor, post:movie` permissions in addition to all the permissions that `User` role has
- Admin
  - can perform all the actions that `Manager` can
  - can also delete an actor or a movie
  - has `delete:actor, delete:movie` permissions in addition to all the permissions that `Manager` role has


## Error Handling
Errors are returned as JSON objects in the following format:
```
{
    "error": 404,
    "message": "The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.",
    "success": false
}
```

The API will return the following errors based on how the request fails:
 - 400: Bad Request
 - 401: Unauthorized
 - 403: Forbidden
 - 404: Not Found
 - 405: Method Not Allowed
 - 422: Unprocessable Entity
 - 500: Internal Server Error

## Endpoints

#### GET /
 - General
   - root endpoint
   - can also work to check if the api is up and running
   - is a public endpoint, requires no authentication
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com`

<details>
<summary>Sample Response</summary>

```
{
    "health": "Running!!"
}
```

</details>

#### GET /actors
 - General
   - gets the list of all the actors
   - requires `get:actors` permission
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/actors`

<details>
<summary>Sample Response</summary>

```
{
    "actors": [
        {
            "id": 1,
            "name": "Anne Hathaway"
        },
        {
            "id": 2,
            "name": "Matthew McConaughey"
        },
        {
            "id": 3,
            "name": "Margot Robbie"
        },
        {
            "id": 4,
            "name": "Mary Elizabeth Winstead"
        }
    ],
    "success": true
}
```

</details>

#### GET /actors/{actor_id}
 - General
   - gets the complete info for an actor
   - requires `get:actors-info` permission
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/actors/1`

<details>
<summary>Sample Response</summary>

```
{
    "actor": {
        "date_of_birth": "November 12, 1982",
        "full_name": "Anne Jacqueline Hathaway",
        "movies": [
            "Serenity"
        ],
        "name": "Anne Hathaway"
    },
    "success": true
}
```
  
</details>

#### POST /actors
 - General
   - creates a new actor
   - requires `post:actor` permission
 
 - Request Body
   - name: string, required
   - full_name: string, optional
   - date_of_birth: date, required
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/actors`
   - Request Body
     ```
        {
            "name": "Ana de Armas",
            "full_name": "Ana Celia de Armas Caso",
            "date_of_birth": "April 30, 1988"
        }
     ```

<details>
<summary>Sample Response</summary>

```
{
    "created_actor_id": 5,
    "success": true
}
```
  
</details>

#### PATCH /actors/{actor_id}
 - General
   - updates the info for an actor
   - requires `patch:actor` permission
 
 - Request Body (at least one of the following fields required)
   - name: string, optional
   - full_name: string, optional
   - date_of_birth: date, optional
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/actors/5`
   - Request Body
     ```
       {
            "full_name": "Ana de Arams Caso"
       }
     ```

<details>
<summary>Sample Response</summary>

```
{
    "actor_info": {
        "date_of_birth": "April 30, 1988",
        "full_name": "Ana de Arams Caso",
        "name": "Ana de Armas"
    },
    "success": true
}
```
  
</details>

#### DELETE /actors/{actor_id}
 - General
   - deletes the actor
   - requires `delete:actor` permission
   - will also delete the mapping to the movie but will not delete the movie from the database
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/actors/5`

<details>
<summary>Sample Response</summary>

```
{
    "deleted_actor_id": 5,
    "success": true
}
```
  
</details>

#### GET /movies
 - General
   - gets the list of all the movies
   - requires `get:movies` permission
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/movies`

<details>
<summary>Sample Response</summary>

```
{
    "movies": [
        {
            "id": 1,
            "release_year": 2019,
            "title": "Serenity"
        },
        {
            "id": 2,
            "release_year": 2020,
            "title": "Birds of Prey"
        }
    ],
    "success": true
}
```

</details>

#### GET /movies/{movie_id}
 - General
   - gets the complete info for a movie
   - requires `get:movies-info` permission
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/movies/1`

<details>
<summary>Sample Response</summary>

```
{
    "movie": {
        "cast": [
            "Anne Hathaway",
            "Matthew McConaughey"
        ],
        "duration": 106,
        "imdb_rating": 5.3,
        "release_year": 2019,
        "title": "Serenity"
    },
    "success": true
}
```
  
</details>

#### POST /movies
 - General
   - creates a new movie
   - requires `post:movie` permission
 
 - Request Body
   - title: string, required
   - duration: integer, required
   - release_year: integer, required
   - imdb_rating: float, required
   - cast: array of string, non-empty, required
 
 - NOTE
   - Actors passed in the `cast` array in request body must already exist in the database prior to making this request.
   - If not, the request will fail with code 422.
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/actors`
   - Request Body
     ```
        {
            "title": "Knives Out",
            "duration": 130,
            "release_year": 2019,
            "imdb_rating": 7.9,
            "cast": ["Ana de Armas"]
        }
     ```

<details>
<summary>Sample Response</summary>

```
{
    "created_movie_id": 3,
    "success": true
}
```
  
</details>

#### PATCH /movie/{movie_id}
 - General
   - updates the info for a movie
   - requires `patch:movie` permission
 
 - Request Body (at least one of the following fields required)
   - title: string, optional
   - duration: integer, optional
   - release_year: integer, optional
   - imdb_rating: float, optional
   - cast: array of string, non-empty, optional
 
 - NOTE
   - Actors passed in the `cast` array in request body will completely replace the existing relationship.
   - So, if you want to append new actors to a movie, pass the existing actors also in the request.
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/movies/3`
   - Request Body
     ```
       {
            "imdb_rating": 8.1
       }
     ```

<details>
<summary>Sample Response</summary>

```
{
    "movie_info": {
        "duration": 130,
        "imdb_rating": 8.1,
        "release_year": 2019,
        "title": "Knives Out"
    },
    "success": true
}
```
  
</details>

#### DELETE /movies/{movie_id}
 - General
   - deletes the movie
   - requires `delete:movie` permission
   - will not affect the actors present in the database
 
 - Sample Request
   - `https://paro-capstone.herokuapp.com/movies/3`

<details>
<summary>Sample Response</summary>

```
{
    "deleted_movie_id": 3,
    "success": true
}
```
  
</details>

## Testing
For testing the backend, run the following commands (in the exact order):
```
dropdb capstone_test
createdb capstone_test
psql capstone_test < casting.sql
python test.py
```

Alternate way: Create the db `capstone_test` using PgAdmin and copy the contents of casting.sql and paste them
in Query tool in PgAdmin and create the db table with records. Then, run the command `python test.py`.



## Deployment
This app is deployed on Heroku. For deployment, you need to:

1. Install Heroku CLI and login to Heroku on the terminal

2. create a [setup.sh](setup.sh) file and declare all your variables in the file

3. Install gunicorn
``` bash
    pip install gunicorn
```

4. Create a [Procfile](Procfile) and add the line below. The Procfile instructs Heroku on what to do. Make sure that **your app** is housed in **[app.py](app.py)**
``` bash
    web: gunicorn app:app
```

5. Install the following requirements
``` bash
    pip install flask_script
    pip install flask_migrate
    pip install psycopg2-binary
```       

6. Freeze your requirements in the [requirements.txt](requirements.txt) file
``` bash
    pip freeze > requirements.txt
```   

7. Create Heroku app
``` bash
    heroku create name_of_your_app
```
        
8. Add git remote for Heroku to local repository
``` bash
    git remote add heroku heroku_git_url
``` 

9. Add postgresql add on for our database
``` bash
    heroku addons:create heroku-postgresql:hobby-dev --app name_of_your_application
```
10. Add all the Variables in Heroku under settings
``` bash
    # This should already exist from the last step
    DATABASE_URL
    # Get these from Auth0
    AUTH0_DOMAIN
    ALGORITHMS
    API_AUDIENCE
```
        
10. Push any changes to your GitHub Repository

11. Push to Heroku
``` bash
    git push heroku master
```      

12. Run Migrations to the Heroku database
``` bash
    heroku run python manage.py db upgrade --app name_of_your_application
```

Visit your Heroku app on the hosted URL!