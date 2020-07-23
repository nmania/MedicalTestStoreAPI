# Medical Data Store Backend

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by naviging to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

## Deployed server

The application is deployed to Heroku and can be accessed on:

```https://lit-tor-15696.herokuapp.com```

To quickly verify that the application is up and running, use the `/` endpoint. It should return a "Hello there" answer:

```json
{
    "success": true,
    "error": 0,
    "message": "Hello there!"
}
```

This is the only endpoint that does not require authentication.

## Running the server locally

From within the root directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
python manage.py run
```

## Authentication

Most of the API endpoints require authentication from the user.

For successful authentication, you have to provide a Bearer token in the Authentication header. Example:

```curl -H "Authorization: Bearer ${TOKEN}" {URL}/{endpoint}```

Sample access tokens for various authorization levels will be provided on project delivery.

## Testing

From within the root directory first ensure you are working using your created virtual environment.

In order to successfully run the tests, you have to set up environmental variables for the access tokens of the different authorization levels. On Windows:
```
export BASIC_TOKEN={token with basic authorization level}
export ADVANCED_TOKEN={token with elevated authorization level}
```

To run the tests, execute:

```bash
python manage.py test
```

## API description

#### Endpoints

1. GET '/'
2. GET '/profile'
3. PATCH '/profile'
4. GET '/tests'
5. POST '/tests'
6. GET '/results'
7. POST '/results'
8. PATCH '/results/<result_id>'
9. DElETE '/results/<result_id>'
10. GET '/users'
11. GET '/users/<user_id>/results'

#### GET '/'

Description: Test endpoint.

Parameters: None

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": "Hello there!"
}
```

Errors: None

#### GET '/profile'

Description: Retrieves the users profile information.

Parameters: None

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message":
    {
        "id": 0,
        "name": "name",
        "tests": 
        [
            {
                "id": 0,
                "name": "name"
            }
        ]
    } 
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403

#### PATCH '/profile'

Description: Updates the profile of the user.

Parameters: 
1. Name

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message":
    {
        "id": 0,
        "name": "name",
        "tests": 
        [
            {
                "id": 0,
                "name": "name"
            }
        ]
    } 
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403
3. Required parameter is not provided: 422

#### GET '/tests'

Description: Retrieved all available test types.

Parameters: None

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 
    [
        {
            "id": 0,
            "name": ""
        }       
    ]
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403

#### POST '/tests'

Description: Registers a new test type.

Parameters:
1. name

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 
    {
        "id": 0,
        "name": ""
    } 
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403
3. Name parameter is missing: 422
4. A test with the requested name already exists: 422

#### GET '/results'

Description: Retrieved all results of the user. (If test_id is defined, only returns the results of that test type.)

Parameters:
1. test_id (optional)

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 
    {
        "user_id": 0,
        "test_id": 0,
        "data": 
            [
                {
                    "id": 0,
                    "test_id": 0,
                    "user_id": 0,
                    "time": "<datetime>",
                    "value": 0.0
                }         
            ]           
    }
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403
3. test_id is defined and is not registered in the database: 404

#### POST '/results'

Description: Registers a single test result under the user's ID.

Parameters:
1. time
2. value
3. test_id

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 
    {
        "id": 0,
        "test_id": 0,
        "user_id": 0,
        "time": "<datetime>",
        "value": 0.0
    } 
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403
3. At least one of the required parameters is missing: 422
4. test_id is not registered in the database: 404

#### PATCH '/results/<result_id>'

Description: Updates a single test result with a new timestamp or value.

Parameters: At least one of the following:
1. time
2. value

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 
    {
        "id": 0,
        "test_id": 0,
        "user_id": 0,
        "time": "<datetime>",
        "value": 0.0
    } 
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403
3. neither time nor value parameters are defined: 422
4. result_id is not registered in the database: 404
5. result_id is registered under a different user: 403

#### DElETE '/results/<result_id>'

Description: Deletes a single test result.

Parameters: None

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 0
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403
3. result_id is not registered in the database: 404
4. result_id is registered under a different user: 403

#### GET '/users'

Description: Get the list of registered users.

Parameters: None

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 
    [
        {
            "id": 0,
            "name": "name",
            "tests": 
            [
                {
                    "id": 0,
                    "name": "name"
                }
            ]
        }          
    ]
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403

#### GET '/users/<user_id>/results'

Description: Get the list of registered test results of a user.

Parameters: 
1. test_id:
    - Int
    - ID of the test you want to get the results of
    - Default: -1 (all tests)

Expected result:
```json
{
    "success": true,
    "error": 0,
    "message": 
    {
        "user_id": 0,
        "test_id": 0,
        "data": 
            [
                {
                    "id": 0,
                    "test_id": 0,
                    "user_id": 0,
                    "time": "<datetime>",
                    "value": 0.0
                }         
            ]           
    }
}
```

Errors: 
1. Authentication header missing: 401
2. Bearer token does not contain required permission: 403
3. user_id does not exist in database: 404
4. test_id is not "-1" and does not exist in database: 404