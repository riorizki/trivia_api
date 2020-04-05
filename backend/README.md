# Full Stack Trivia API Backend

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Enviornment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by naviging to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py.

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server.

## Database Setup

With Postgres running, restore a database using the trivia.psql file provided. From the backend folder in terminal run:

```bash
psql trivia < trivia.psql
```

## Running the server

From within the `backend` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
export FLASK_APP=flaskr
export FLASK_ENV=development
flask run
```

Setting the `FLASK_ENV` variable to `development` will detect file changes and restart the server automatically.

Setting the `FLASK_APP` variable to `flaskr` directs flask to use the `flaskr` directory and the `__init__.py` file to find the application.

## Tasks

One note before you delve into your tasks: for each endpoint you are expected to define the endpoint and response data. The frontend will be a plentiful resource because it is set up to expect certain endpoints and response data formats already. You should feel free to specify endpoints in your own way; if you do so, make sure to update the frontend or you will get some unexpected behavior.

1. Use Flask-CORS to enable cross-domain requests and set response headers.
2. Create an endpoint to handle GET requests for questions, including pagination (every 10 questions). This endpoint should return a list of questions, number of total questions, current category, categories.
3. Create an endpoint to handle GET requests for all available categories.
4. Create an endpoint to DELETE question using a question ID.
5. Create an endpoint to POST a new question, which will require the question and answer text, category, and difficulty score.
6. Create a POST endpoint to get questions based on category.
7. Create a POST endpoint to get questions based on a search term. It should return any questions for whom the search term is a substring of the question.
8. Create a POST endpoint to get questions to play the quiz. This endpoint should take category and previous question parameters and return a random questions within the given category, if provided, and that is not one of the previous questions.
9. Create error handlers for all expected errors including 400, 404, 422 and 500.

## API Reference

### Getting Started

- Base URL: Currently this application is only hosted locally. The backend is hosted at `http://127.0.0.1:5000/`
- Authentication: This version does not require authentication or API keys.

### Error Handling

Errors are returned as JSON in the following format:<br>

    {
        "success": False,
        "error": 404,
        "message": "resource not found"
    }

The API will return three types of errors:

- 400 – bad request
- 404 – resource not found
- 422 – unprocessable

### Endpoints

#### GET /categories

- General: Returns a list categories.
- Sample: `curl http://127.0.0.1:5000/categories`<br>

        {
            "categories": {
                "1": "Science",
                "2": "Art",
                "3": "Geography",
                "4": "History",
                "5": "Entertainment",
                "6": "Sports"
            },
            "success": true
        }

#### GET /questions

- General:
  - Returns a list questions.
  - Results are paginated in groups of 10.
  - Also returns list of categories and total number of questions.
- Sample: `curl http://127.0.0.1:5000/questions`<br>

        {
            "categories": {
                "1": "Science",
                "2": "Art",
                "3": "Geography",
                "4": "History",
                "5": "Entertainment",
                "6": "Sports"
            },
            "questions": [
                {
                    "answer": "Colorado, New Mexico, Arizona, Utah",
                    "category": 3,
                    "difficulty": 3,
                    "id": 164,
                    "question": "Which four states make up the 4 Corners region of the US?"
                },
                {
                    "answer": "Muhammad Ali",
                    "category": 4,
                    "difficulty": 1,
                    "id": 9,
                    "question": "What boxer's original name is Cassius Clay?"
                },
                {
                    "answer": "Apollo 13",
                    "category": 5,
                    "difficulty": 4,
                    "id": 2,
                    "question": "What movie earned Tom Hanks his third straight Oscar nomination, in 1996?"
                },
                {
                    "answer": "Tom Cruise",
                    "category": 5,
                    "difficulty": 4,
                    "id": 4,
                    "question": "What actor did author Anne Rice first denounce, then praise in the role of her beloved Lestat?"
                },
                {
                    "answer": "Edward Scissorhands",
                    "category": 5,
                    "difficulty": 3,
                    "id": 6,
                    "question": "What was the title of the 1990 fantasy directed by Tim Burton about a young man with multi-bladed appendages?"
                },
                {
                    "answer": "Brazil",
                    "category": 6,
                    "difficulty": 3,
                    "id": 10,
                    "question": "Which is the only team to play in every soccer World Cup tournament?"
                },
                {
                    "answer": "Uruguay",
                    "category": 6,
                    "difficulty": 4,
                    "id": 11,
                    "question": "Which country won the first ever soccer World Cup in 1930?"
                },
                {
                    "answer": "George Washington Carver",
                    "category": 4,
                    "difficulty": 2,
                    "id": 12,
                    "question": "Who invented Peanut Butter?"
                },
                {
                    "answer": "Lake Victoria",
                    "category": 3,
                    "difficulty": 2,
                    "id": 13,
                    "question": "What is the largest lake in Africa?"
                },
                {
                    "answer": "The Palace of Versailles",
                    "category": 3,
                    "difficulty": 3,
                    "id": 14,
                    "question": "In which royal palace would you find the Hall of Mirrors?"
                }
            ],
            "success": true,
            "total_questions": 19
        }

#### DELETE /questions/\<int:id\>

- General:
  - Deletes a question by id using url parameters.
  - Returns id of deleted question upon success.
- Sample: `curl http://127.0.0.1:5000/questions/6 -X DELETE`<br>

        {
            "deleted": 6,
            "success": true
        }

#### POST /questions

This endpoint either creates a new question or returns search results.

1. If <strong>no</strong> search term is included in request:

- General:
  - Creates a new question using JSON request parameters.
  - Returns JSON object with newly created question, as well as paginated questions.
- Sample: `curl http://127.0.0.1:5000/questions -X POST -H "Content-Type: application/json" -d '{ "question": "Which US state contains an area known as the Upper Penninsula?", "answer": "Michigan", "difficulty": 3, "category": "3" }'`<br>

        {
            "created": 173,
            "question_created": "Which US state contains an area known as the Upper Penninsula?",
            "questions": [
                {
                    "answer": "Apollo 13",
                    "category": 5,
                    "difficulty": 4,
                    "id": 2,
                    "question": "What movie earned Tom Hanks his third straight Oscar nomination, in 1996?"
                },
                {
                    "answer": "Tom Cruise",
                    "category": 5,
                    "difficulty": 4,
                    "id": 4,
                    "question": "What actor did author Anne Rice first denounce, then praise in the role of her beloved Lestat?"
                },
                {
                    "answer": "Muhammad Ali",
                    "category": 4,
                    "difficulty": 1,
                    "id": 9,
                    "question": "What boxer's original name is Cassius Clay?"
                },
                {
                    "answer": "Brazil",
                    "category": 6,
                    "difficulty": 3,
                    "id": 10,
                    "question": "Which is the only team to play in every soccer World Cup tournament?"
                },
                {
                    "answer": "Uruguay",
                    "category": 6,
                    "difficulty": 4,
                    "id": 11,
                    "question": "Which country won the first ever soccer World Cup in 1930?"
                },
                {
                    "answer": "George Washington Carver",
                    "category": 4,
                    "difficulty": 2,
                    "id": 12,
                    "question": "Who invented Peanut Butter?"
                },
                {
                    "answer": "Lake Victoria",
                    "category": 3,
                    "difficulty": 2,
                    "id": 13,
                    "question": "What is the largest lake in Africa?"
                },
                {
                    "answer": "The Palace of Versailles",
                    "category": 3,
                    "difficulty": 3,
                    "id": 14,
                    "question": "In which royal palace would you find the Hall of Mirrors?"
                },
                {
                    "answer": "Agra",
                    "category": 3,
                    "difficulty": 2,
                    "id": 15,
                    "question": "The Taj Mahal is located in which Indian city?"
                },
                {
                    "answer": "Escher",
                    "category": 2,
                    "difficulty": 1,
                    "id": 16,
                    "question": "Which Dutch graphic artist\u2013initials M C was a creator of optical illusions?"
                }
            ],
            "success": true,
            "total_questions": 20
        }

2. If search term <strong>is</strong> included in request:

- General:
  - Searches for questions using search term in JSON request parameters.
  - Returns JSON object with paginated matching questions.
- Sample: `curl http://127.0.0.1:5000/questions -X POST -H "Content-Type: application/json" -d '{"searchTerm": "which"}'`<br>

        {
            "questions": [
                {
                    "answer": "Brazil",
                    "category": 6,
                    "difficulty": 3,
                    "id": 10,
                    "question": "Which is the only team to play in every soccer World Cup tournament?"
                },
                {
                    "answer": "Uruguay",
                    "category": 6,
                    "difficulty": 4,
                    "id": 11,
                    "question": "Which country won the first ever soccer World Cup in 1930?"
                },
                {
                    "answer": "The Palace of Versailles",
                    "category": 3,
                    "difficulty": 3,
                    "id": 14,
                    "question": "In which royal palace would you find the Hall of Mirrors?"
                },
                {
                    "answer": "Agra",
                    "category": 3,
                    "difficulty": 2,
                    "id": 15,
                    "question": "The Taj Mahal is located in which Indian city?"
                },
                {
                    "answer": "Escher",
                    "category": 2,
                    "difficulty": 1,
                    "id": 16,
                    "question": "Which Dutch graphic artist\u2013initials M C was a creator of optical illusions?"
                },
                {
                    "answer": "Jackson Pollock",
                    "category": 2,
                    "difficulty": 2,
                    "id": 19,
                    "question": "Which American artist was a pioneer of Abstract Expressionism, and a leading exponent of action painting?"
                },
                {
                    "answer": "Scarab",
                    "category": 4,
                    "difficulty": 4,
                    "id": 23,
                    "question": "Which dung beetle was worshipped by the ancient Egyptians?"
                },
                {
                    "answer": "Michigan",
                    "category": 3,
                    "difficulty": 3,
                    "id": 173,
                    "question": "Which US state contains an area known as the Upper Penninsula?"
                }
            ],
            "success": true,
            "total_questions": 18
        }

#### GET /categories/\<int:id\>/questions

- General:
  - Gets questions by category id using url parameters.
  - Returns JSON object with paginated matching questions.
- Sample: `curl http://127.0.0.1:5000/categories/1/questions`<br>

        {
            "current_category": "Science",
            "questions": [
                {
                    "answer": "The Liver",
                    "category": 1,
                    "difficulty": 4,
                    "id": 20,
                    "question": "What is the heaviest organ in the human body?"
                },
                {
                    "answer": "Alexander Fleming",
                    "category": 1,
                    "difficulty": 3,
                    "id": 21,
                    "question": "Who discovered penicillin?"
                },
                {
                    "answer": "Blood",
                    "category": 1,
                    "difficulty": 4,
                    "id": 22,
                    "question": "Hematology is a branch of medicine involving the study of what?"
                }
            ],
            "success": true,
            "total_questions": 18
        }

#### POST /quizzes

- General:
  - Allows users to play the quiz game.
  - Uses JSON request parameters of category and previous questions.
  - Returns JSON object with random question not among previous questions.
- Sample: `curl http://127.0.0.1:5000/quizzes -X POST -H "Content-Type: application/json" -d '{"previous_questions": [20, 21], "quiz_category": {"type": "Science", "id": "1"}}'`<br>

        {
            "question": {
                "answer": "Blood",
                "category": 1,
                "difficulty": 4,
                "id": 22,
                "question": "Hematology is a branch of medicine involving the study of what?"
            },
            "success": true
        }

## Testing

To run the tests, run

```
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```
