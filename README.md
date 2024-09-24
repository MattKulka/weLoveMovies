# WeLoveMovies Backend & API

- [Live Link](https://movie-frontend-bnfp.onrender.com/)  (Please allow time for data to populate )

The backend and API of WeLoveMovies facilitates storage and delivery of WeLoveMovies' movies, theaters, and reviews data. 

This assignment required:
- Installation and use of common middleware packages
- Use of routes to receive requests
- Access to relevant information through route and query parameters
- Creation of error handlers
- An API that follows RESTful design principles
- Creation and customization of a knexfile.js
- Use of Knex to connect to my PostgreSQL database
- Database queries to complete CRUD routes in an Express server
- Return of joined and nested data with Knex
- Routes that respond with the appropriate status code and include a data key in the response
- Creation of database migrations using Knex's migration tool
- Deployment of my backend server to a cloud service (Heroku)
- Use of the CORS package so that requests from the frontend can correctly reach the backend

## Technology
Built with:
- Node.js
  - Express server framework
  - CORS package
- PostgreSQL database
  - Knex.js for query building

## Screenshots:

![Screenshot 2023-12-19 154810](https://github.com/MattKulka/weLoveMovies/assets/92747257/dc7f7b12-1333-47da-bee0-e85c4307c651)
![Screenshot 2023-12-19 154904](https://github.com/MattKulka/weLoveMovies/assets/92747257/807b884e-399e-496a-9b94-33f8c734d1b9)
![Screenshot 2023-12-19 154942](https://github.com/MattKulka/weLoveMovies/assets/92747257/335c0bc8-7529-4cbd-9a44-1ee2c14b9769)
![Screenshot 2023-12-19 155021](https://github.com/MattKulka/weLoveMovies/assets/92747257/0c3e2f28-e259-44b7-93bc-292ba884307f)
![Screenshot 2023-12-19 155052](https://github.com/MattKulka/weLoveMovies/assets/92747257/58686712-dd2f-4465-aaf5-15f5b3be87b8)

### Endpoints for movies:
**Get Movies:** GET to `/movies`
- Requests all movies in the database.
- In the event that `is_showing=true` is provided, the route will return only those movies that are currently showing in theaters.
- Successful GET requests will return an array of JSON objects representing the saved movies. Each movie object contains the following fields:
  - `id`: integer
  - `title`: string
  - `runtime_in_minutes`: integer
  - `rating`: string
  - `description`: text
  - `image_url`: string

**Get Movie by ID:** GET to `/movies/:movieId`
- Requests a specific movie by ID.
- Successful GET request will return a JSON object with the same fields as above.
- Note: If the given ID does not match an existing movie, a response like the following should be returned:
```
{
  "error": "Movie cannot be found."
}
```

**Get Theaters Showing Movie:** GET to `/movies/:movieId/theaters`
- Requests all the theaters the movie is playing at.
- Successful GET request will return an array of JSON objects representing the theaters. Each theater object contains the following fields:
  - `theater_id`: integer
  - `name`: string
  - `address_line_1`: string
  - `address_line_2`: string
  - `city`: string
  - `state`: string
  - `zip`: string
  - `created_at`: date
  - `updated_at`: date
  - `is_showing`: boolean
  - `movie_id`: integer

**Get Reviews for a Movie:** GET to `/movies/:movieId/reviews`
- Requests all of the reviews for a movie, and each review object contains a `"critic"` key with the critic's details as its value. 
- Successful GET request will return an array of JSON objects representing the reviews. The response from the server should look like the following:
```
{
  "data": [
    {
      "review_id": 1,
      "content": "Lorem markdownum ...",
      "score": 3,
      "created_at": "2021-02-23T20:48:13.315Z",
      "updated_at": "2021-02-23T20:48:13.315Z",
      "critic_id": 1,
      "movie_id": 1,
      "critic": {
        "critic_id": 1,
        "preferred_name": "Chana",
        "surname": "Gibson",
        "organization_name": "Film Frenzy",
        "created_at": "2021-02-23T20:48:13.308Z",
        "updated_at": "2021-02-23T20:48:13.308Z"
      }
    }
    // ...
  ]
}
```

### Endpoints for theaters:
**Get Theaters:** GET to `/theaters`
- Requests all theaters in the database, and each theater object contains a `"movies"` key with the movies playing at that theater as its value.
- The response from the server should look like the following:
```
{
  "data": [
    {
      "theater_id": 1,
      "name": "Regal City Center",
      "address_line_1": "801 C St.",
      "address_line_2": "",
      "city": "Vancouver",
      "state": "WA",
      "zip": "98660",
      "created_at": "2021-02-23T20:48:13.335Z",
      "updated_at": "2021-02-23T20:48:13.335Z",
      "movies": [
        {
          "movie_id": 1,
          "title": "Spirited Away",
          "runtime_in_minutes": 125,
          "rating": "PG",
          "description": "Chihiro...",
          "image_url": "https://imdb-api.com...",
          "created_at": "2021-02-23T20:48:13.342Z",
          "updated_at": "2021-02-23T20:48:13.342Z",
          "is_showing": false,
          "theater_id": 1
        }
        // ...
      ]
    }
    // ...
  ]
}
```

### Endpoints for reviews:
**Update Review:** PUT to `/reviews/:reviewId`
- PUT request will be sent with a single JSON object like so:
```
{
  "score": 3,
  "content": "New content..."
}
```
- The response from the server should include the entire review record with the newly patched content, and the critic information set to the `critic` property.
```
{
  "data": {
    "review_id": 1,
    "content": "New content...",
    "score": 3,
    "created_at": "2021-02-23T20:48:13.315Z",
    "updated_at": "2021-02-23T20:48:13.315Z",
    "critic_id": 1,
    "movie_id": 1,
    "critic": {
      "critic_id": 1,
      "preferred_name": "Chana",
      "surname": "Gibson",
      "organization_name": "Film Frenzy",
      "created_at": "2021-02-23T20:48:13.308Z",
      "updated_at": "2021-02-23T20:48:13.308Z"
    }
  }
}
```
- Note: If the given ID does not match an existing review, a response, like the following, should be returned with a response status code of `404`:
```
{
  "error": "Review cannot be found."
}
```

**Delete Review by ID:** DELETE to `/reviews/:reviewId`
- DELETE request will be sent without a request body.
- Note: If the given ID does not match an existing review, a response, like the following, should be returned with a response status code of `404`:
```
{
  "error": "Review cannot be found."
}
```
