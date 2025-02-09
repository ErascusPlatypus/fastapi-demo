# fastapi-demo

This is a FastAPI demo project that interacts with a PostgreSQL database, providing CRUD (Create, Read, Update, Delete) operations for movie data.

## Prerequisites

- Python 3.7+ 
- PostgreSQL installed on your machine.

## Steps to Run

### 1. Install Dependencies

Start by installing the necessary Python dependencies.

- **PostgreSQL dependencies:**

   ```bash
   pip install 'databases[postgresql]'
   ```

- **All project dependencies** (including FastAPI, SQLAlchemy, and others):

   ```bash
   pip install -r requirements.txt
   ```

### 2. Set Up PostgreSQL Database

Before running the FastAPI application, follow these steps to set up your PostgreSQL database:

1. **Switch to the `postgres` user**:

   ```bash
   sudo -u postgres psql
   ```

2. **Create and connect to your `movie_db` database**:

   ```sql
   CREATE DATABASE movie_db;
   \c movie_db
   ```

3. **Grant necessary permissions to the `movie_user` for the schema** (if you have a dedicated user):

   ```sql
   GRANT ALL ON SCHEMA public TO movie_user;
   ```

   If you haven't set up a `movie_user`, you can either modify your `db_manager` to work with the default `postgres` user, or create a new user:

   ```sql
   CREATE USER movie_user WITH PASSWORD 'your_password';
   GRANT ALL PRIVILEGES ON DATABASE movie_db TO movie_user;
   ```

### 2. Set Up PostgreSQL Database
Before running the application, follow these steps to set up your PostgreSQL database:

1. Switch to the `postgres` user:

   ```bash
   sudo -u postgres psql
   ```

2. Connect to your `movie_db` database:

   ```sql
   \c movie_db postgres
   ```

3. Grant necessary permissions to the `movie_user` for the schema:

   ```sql
   GRANT ALL ON SCHEMA public TO movie_user;
   ```

### 3. Run the Application

Once the PostgreSQL setup is complete and dependencies are installed, you can start the FastAPI application using **Uvicorn**:

```bash
uvicorn app.main:app --reload
```

This will start the FastAPI application, and you should be able to access it via `http://127.0.0.1:8000`.

### 4. Interacting with the API

#### Request Body for Adding a Movie

Here’s the structure of the request body to add a new movie to the database:

```json
{
  "name": "Oppenheimer",
  "plot": "nukkkkkkkkkkkessssssssssss",
  "genres": ["thriller"],
  "casts": ["Robert Downey Jr"]
}
```

#### Sample cURL Request to Add a Movie

You can use `cURL` to interact with the API:

```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "Oppenheimer",
  "plot": "nukkkkkkkkkkkessssssssssss",
  "genres": [
    "thriller"
  ],
  "casts": [
    "Robert Downey Jr"
  ]
}'
```

#### Sample cURL Request to Get All Movies

```bash
curl -X 'GET' \
  'http://127.0.0.1:8000/' \
  -H 'accept: application/json'
```

#### Sample cURL Request to Get Movie Count

To get the total number of movies in the database, you can use the following cURL request:

```bash
curl -X 'GET' \
  'http://127.0.0.1:8000/count' \
  -H 'accept: application/json'
```

### Available Endpoints

- **GET `/`** – Get all movies in the database.
- **POST `/`** – Add a new movie to the database.
- **PUT `/{id}`** – Update an existing movie by ID.
- **DELETE `/{id}`** – Delete a movie by ID.
- **GET `/count`** – Get the total count of movies in the database.

### Example Responses

- **GET `/` Response:**

   ```json
   [
     {
       "id": 1,
       "name": "Oppenheimer",
       "plot": "nukkkkkkkkkkkessssssssssss",
       "genres": ["thriller"],
       "casts": ["Robert Downey Jr"]
     },
     {
       "id": 2,
       "name": "Avatar",
       "plot": "A journey through a fantastic world.",
       "genres": ["action", "adventure"],
       "casts": ["Sam Worthington", "Zoe Saldana"]
     }
   ]
   ```

- **GET `/count` Response:**

   ```json
   {
     "count": 42
   }
   ```

### Additional Information

- The project uses **SQLAlchemy** (or a database manager) to interact with PostgreSQL asynchronously.
- You can modify the `db_manager` file to add more features like handling database migrations, etc.
- The database configuration should be correctly set up to match the `db_manager` logic.

### Troubleshooting

- **Permission Denied Error:**
  If you encounter a `permission denied for schema public` error, make sure the `movie_user` has the correct privileges to access and modify the database schema. Run the following commands in the PostgreSQL shell:

   ```sql
   GRANT ALL ON SCHEMA public TO movie_user;
   GRANT ALL ON ALL TABLES IN SCHEMA public TO movie_user;
   ```

- **Missing Tables or Columns:**
  If you encounter issues related to missing tables or columns, ensure that your models match the database schema, or consider adding a migration step using a library like Alembic.

