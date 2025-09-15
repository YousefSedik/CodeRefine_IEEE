## Functional Requirements

1. Administrator should add new movies.
2. Administrator should add mange content.
3. Users should track their watch history.
4. Users can add reviews & rating.
5. Users can report content.
6. Administrator can add the following movie metadata(titles, descriptions, video trailers, cast members).
7. System should support finding movies by title, actor, genre.
8. Users can add a top 10 movies (for each genre) to their personal profile page.
9. System should recommend similar movies to the user
10. System should push notification to users whenever there’s a new movie the user might like.

## Non Functional Requirements

1. The Search Engine should respond in less than 1 second.
2. the system must be always available.
3. the user interface must be user friendly.
4. The System should be able to manage information about millions of movies, including titles, descriptions, video trailers, cast members.

# Data Model

- you can find the data model in the file named `Data Model.png'

# API Design

### Movie Management

#### admin add movie

POST /api/v1/movie

- Request Body:
  ```json
  {
    "title": "title",
    "description": "description",
    "video_trailer": "path/to/trailer.mp4",
    "movie": "path/to/movie.mp4",
    "admin_uploader_id": 123,
    "Genres": ["Action", "Comedy"],
    "cast_members": [1, 2]
  }
  ```

#### admin update movie

PUT /api/v1/movie/{movie_id}

- Request Body:
  ```json
  {
    "title": "Movie Title",
    "description": "Movie Description",
    "video_trailer": "path/to/trailer.mp4",
    "movie": "path/to/movie.mp4",
    "admin_uploader_id": 123,
    "Genres": ["Action", "Comedy"]
  }
  ```

#### get movie by id

GET /api/v1/movie/{movie_id}

- Response Body:
  ```json
  {
    "movie_id": 1,
    "title": "Movie Title",
    "description": "Movie Description",
    "video_trailer": "path/to/trailer.mp4",
    "movie": "path/to/movie.mp4",
    "admin_uploader_id": 123,
    "Genres": ["Action", "Comedy"],
    "cast_members": [
      {
        "id": 1,
        "name": "name",
        "profile_picture": "path/to/picture"
      },
      {
        "id": 1,
        "name": "name",
        "profile_picture": "path/to/picture"
      }
    ],
    "average_participated_movie_rating": 4.5
  }
  ```

#### search movies

GET /api/v1/movie/search

- Query Parameters:
  - title (optional)
  - actor (optional)
  - genre (optional)
- Response Body:
  ```json
  [
    {
      "movie_id": 1,
      "title": "Movie Title",
      "description": "Movie Description",
      "rating": 4.5,
      "Genres": [1, 2]
    }
  ]
  ```

### Report Content

#### add report

POST /api/v1/report

- Request Body:
  ```json
  {
    "movie_id": 456,
    "reason": "Inappropriate content"
  }
  ```

#### get all reports (admin only)

GET /api/v1/report

- Response Body:
  ```json
  [
    {
      "report_id": 1,
      "movie_id": 456,
      "user_id": 123,
      "reason": "Inappropriate content",
      "status": "Pending",
      "created_at": "2023-10-01T12:34:56Z"
    }
  ]
  ```

#### update report status (admin only)

PUT /api/v1/report/{report_id}

- Request Body:

  ```json
  {
    "status": "Resolved"
  }
  ```

### User Interactions

#### add review & rating

POST /api/v1/review

- Request Body:
  ```json
  {
    "movie_id": 456,
    "rate": 4,
    "review": "Great movie!"
  }
  ```

#### get reviews for a movie

GET /api/v1/review/{movie_id}

- Response Body:
  ```json
  [
    {
      "review_id": 1,
      "user_id": 123,
      "movie_id": 456,
      "rate": 4,
      "review": "Great movie!",
      "created_at": "2023-10-01T12:34:56Z"
    }
  ]
  ```

#### get watch history

GET /api/v1/history

- Response Body:

  ```json
  [
    {
      "history_id": 1,
      "user_id": 123,
      "movie_id": 456,
      "watched_at": "2023-10-01T12:34:56Z"
    }
  ]
  ```

## Notifications

#### get notifications

GET /api/v1/notifications

- Response Body:

  ```json
  [
    {
      "notification_id": 1,
      "user_id": 123,
      "message": "New movie 'Inception' added that you might like!",
      "is_read": false,
      "created_at": "2023-10-01T12:34:56Z"
    }
  ]
  ```

#### mark notification as read

PUT /api/v1/notifications/{notification_id}

- Request Body:

  ```json
  {
    "is_read": true
  }
  ```
