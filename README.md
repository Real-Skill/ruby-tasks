# Rails JSON API

### Summary
JSON API, routing basic Rails, handling error contollers, and associations

### Getting Started

In order to get started with the task you should run the following commands

To install required gems run:
`bundle install`

In order to create development database run: `bundle exec rake db:create`. This will result in creating sqlite database which is sufficient for this test

Feel free to install any gem that helps you with the development process.



### Goal
Implement JSON API with the following routes:

1. `POST   /movies`
    * returns HTTP 201 and `MOVIE_OBJECT` with root "movie"
    * return HTTP 422 and proper errors object(See ERRORS section) when invalid data has been provided
2. `GET    /movies`
    * returns HTTP 200 and collection of movies as array under "movies" root, and "meta" key containing "count" with number of existing movies - See `MOVIES_COLLECTION`
3. `GET    /movies/:id`
    * returns HTTP 200 and `MOVIE_OBJECT` with "movie" root
    * returns HTTP 404 not found error(See ERRORS section) if the resource has not been found
4. `PUT    /movies/:id`
    * filters rate and votes_count and does not allow to update them
    * returns HTTP 200 and `MOVIE_OBJECT` with "movie" root
    * returns HTTP 404 not found error(See ERRORS section) if the resource has not been found
5. `POST   /movies/:id/rate`
    * returns 200 when valid `RATE_POST_DATA` is provided - {resource}: "movie"
    * returns 422 error for rate field when invalid data has been provided(vote not in range)
6. `POST    /movies/:movie_id/actors/`
    * returns 201 and `ACTOR_ADDED_TO_CAST_OBJECT` when valid actor_id is provided and actor has been associated with movie
    * returns HTTP 404 not found error(See ERRORS section) if any of the resources has not been found
7. `GET    /movies/:movie_id/actors`
    * returns 200 and collection of actors in scope of the movie as an array under "actors" root, and "meta" key containing "count" with number of actors in the movie - See `ACTORS_COLLECTION` - Actors should be sorted by rate and name
8. `POST /actors`
    * returns HTTP 201 and `ACTOR_OBJECT` with "actor" root 
    * returns HTTP 422 and proper errors object(See ERRORS section) when invalid data has been provided
9. `GET /actors`
     * returns 200 and collection of actors as an array under "actors" root, and "meta" key containing "count" with number of actors in the movie - See `ACTORS_COLLECTION`
10. `GET /actors/:id`
    * returns HTTP 200 and `ACTOR_OBJECT` with "movie" root
    * returns HTTP 404 not found error(See ERRORS section) if the resource has not been found
11. `PUT /actors/:id`
    * filters rate and votes_count and does not allow to update them
    * returns HTTP 200 and `ACTOR_OBJECT` with "actor" root
    * returns HTTP 404 not found error(See ERRORS section) if the resource has not been found
12. `POST /movies/:id/rate`
    * returns 200 when valid `RATE_POST_DATA` is provided- {resource}: "actor"
    * returns 422 error for rate field(See ERRORS section) when invalid data has been provided(vote not in range)

All requests should return 400 - BAD_REQUEST error(See ERRORS section) when invalid input data has been provided
All requests should return 406 - REQUEST_NOT_ACCEPTABLE error(See ERRORS section) if request format is not json
Valid POST and PUT data should be nested under a root key just like in `MOVIE_OBJECT` or `ACTOR_OBJECT`

### Resources
##### Movie
Attributes:
* id[Integer]
* title[String]
* description[String]
* year[Datetime/String]
* runtime[Integer]
* rate[Float] - represented as String
* votes_count[Integer]

And should follow the rules listed below:
* the title and description must be present
* the year must be present and in the range: 1900 - Current year + 3
* runtime must be present and the number must be higher than 0
* it can be associated with many actors


##### Actor
Attributes:
* id[Integer]
* name[String]
* date_of_birth[Datetime/String]
* rate[Float] - represented as String
* votes_count[Integer]

It also should follow the rules listed below:
* the name should be present
* the date of birth should be present and in format: "YYYY-MM-DD"


### JSON Objects

`MOVIES_COLLECTION`
```
{
    "movies":  [MOVIE_OBJECT, MOVIE_OBJECT, ...],
    "meta": {
        "count": count[Integer]
    }
}
```


`MOVIE_OBJECT`
```
{
    "id": id[Integer],
    "title": title[String],
    "description": description[String],
    "year": year[String],
    "runtime": runtime[Integer],
    "rate": rate[String],
    "votes_count": votes_count[Integer]
}
```

`RATE_POST_DATA`
* vote - should be in range 0.1 - 10.0
```
{
    {resource}: {
        "vote": vote[Float] - represented as String
    }
}
```


`ACTORS_COLLECTION`

```
{
    "actors":  [ACTOR_OBJECT, ACTOR_OBJECT, ...],
    "meta": {
        "count": count[Integer]
    }
}
```

`ACTOR_OBJECT`

```
{
    "id": id[Integer],
    "name": name[String],
    "date_of_birth": date[String], - format: "YYYY-MM-DD"
    "rate":"0.0",
    "votes_count":0
}
```

`ACTOR_ADDED_TO_CAST_OBJECT`

```
{
    "message": "Actor {actor_name} has beed adden to {movie_title} cast"
}
```



### ERRORS
* code - an application-specific error code
    * VALDATION_ERROR - for validation related errors
    * RESOURCE_NOT_FOUND - for missing resource errors
* status - http status
* message - error message
* field_name[optional] - should be present when validation error occured and it has an invalid field name

```
{  
   "errors": [  
      {  
         "code":code[String],
         "status": http_status[Integer],
         "message": error_message[String],
         "field_name": name_of_invalid_field[String]*
      }
   ]
}
```

###  Notice
Tests do not verify exact messages for errors. They rather verify the required error response structure.

**It is enough to complete the task making changes to app lib and db directories**

### Verify your solution

Install dependencies:

    bundle install

Execute test suite and verify your solution:

    rake verify
