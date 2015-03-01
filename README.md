# flask-swagger
A swagger spec extractor for flask

Install:
````
pip install flask-swagger
````
flask-swagger provides a method (swagger) that inspects the flask app for endpoints that contain YAML docstrings with swagger 2.0 Path objects. 

````
class UserAPI(MethodView):

    def post(self):
        """
        Create a new user
        ---
        tags:
          - users
        parameters:
          - in: body
            name: body
            schema:
              name: User
              required:
                - email
                - name
              properties:
                email:
                  type: string
                  description: email for user
                name:
                  type: string
                  description: name for user
        responses:
          201:
            description: User created
        """
        return {}

````
flask-swagger supports docstrings in MethodView classes and regular flask view functions.

Following YAML conventions, flask-swagger searches for ---, everything preceding is provided as summary (first line) and description (following lines) for the endpoint while everything after is parsed as a swagger Path object.

To expose your swagger specification to the world you provide a flask route that does something along these lines

````
from flask import Flask, jsonify
from flask_swagger import swagger

app = Flask(__name__)

@app.route("/spec")
def spec():
    return jsonify(swagger(app))

````

Note that the swagger specification returned by swagger(app) is as minimal as it can be. It's your job to override and add to the specification as you see fit.
````
@app.route("/spec")
def spec():
    swag = swagger(app)
    swag['info']['version'] = "1.0"
    swag['info']['title'] = "My API"
    return jsonify(swag)

````


Swagger-UI

Swagger-UI is the reason we embarked on this mission to begin with, flask-swagger does not however include Swagger-UI. Simply follow the awesome documentation over at https://github.com/swagger-api/swagger-ui and point your swaggerUi.url to your new flask-swagger endpoint and enjoy.




