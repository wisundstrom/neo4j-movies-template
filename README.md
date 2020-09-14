# README

This Neo4j-based node / react web app displays movie and person data in a manner similar to IMDB.  It is designed to serve as a template for further development projects.  Feel encouraged to fork and update this repo!

## The Model

![image of movie model](./img/model.png)

### Nodes

* `Movie`
* `Person`
* `Genre`

### Relationships

* `(:Person)-[:ACTED_IN {role:"some role"}]->(:Movie)`
* `(:Person)-[:DIRECTED]->(:Movie)`
* `(:Person)-[:WRITER_OF]->(:Movie)`
* `(:Person)-[:PRODUCED]->(:Movie)`
* `(:MOVIE)-[:HAS_GENRE]->(:Genre)`

## Database Setup: Sandbox

Go to https://sandbox.neo4j.com/, pick "Recommendations", and press play to start the database. 
Update the `DATABASE_USERNAME`, 
`DATABASE_PASSWORD`, and `DATABASE_URL` of your chosen backend to connect to your instance.

## Node API

From the root directory of this project:

* `cd api`
* `nvm use`
* `npm install`
* `node app.js` starts the API
* Take a look at the docs at [http://localhost:3000/docs](http://localhost:3000/docs)

## Alternative: Flask API

From the root directory of this project:

```
cd flask-api
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
export FLASK_APP=app.py
flask run
```

* Take a look at the docs at [http://localhost:5000/docs](http://localhost:5000/docs)

## Frontend

From the root directory of this project, set up and start the frontend with:

* `cd web`
* `nvm use`
* `npm install` (if `package.json` changed)
* update config.settings.js file

If you are using the Node API: `copy src/config/settings.example.js src/config/settings.js`

If you are using the flask api then edit `src/config/settings.js` and change the `apiBaseURL` to `http://localhost:5000/api/v0`

* `npm start` starts the app on [http://localhost:3000/](http://localhost:3000/)

![image of PATH settings for NPM](./img/webUX.png)

voilà! Netflix, eat your heart out ;-)

## Ratings and Recommendations

### User-Centric, User-Based Recommendations

Based on my similarity to other users, user `Omar Huffman` might be interested in movies rated highly by users with similar ratings as himself.

```
MATCH (me:User {name:"Omar Huffman"})-[my:RATED]->(m:Movie)
MATCH (other:User)-[their:RATED]->(m)
WHERE me <> other
AND abs(my.rating - their.rating) < 2
WITH other,m
MATCH (other)-[otherRating:RATED]->(movie:Movie)
WHERE movie <> m
WITH avg(otherRating.rating) AS avgRating, movie
RETURN movie
ORDER BY avgRating desc
LIMIT 25
```

## Contributing

### Node.js/Express API

The Express API is located in the `/api` folder.

#### Create Endpoint

The API itself is created using the [Express web framework for Node.js](https://expressjs.com/). The API endpoints
are documented using swagger and [swagger-jsdoc](https://www.npmjs.com/package/swagger-jsdoc) module.

To add a new API endpoint there are 3 steps:

1. Create a new route method in `/api/routes` directory
2. Describe the method with swagger specification inside a JSDoc comment to make it visible in swagger
3. Add the new route method to the list of route methods in `/api/app.js`.

### Flask API

The flask API is located in the flask-api folder.  The application code is in the `app.py` file.

#### Create Endpoint

The API itself is created using the [Flask-RESTful](http://flask-restful-cn.readthedocs.io/en/0.3.5/) library.  The API endpoints
are documented using swagger with the [flask-restful-swagger-2](https://github.com/swege/flask-restful-swagger-2.0) library.

To add a new API endpoint there are 3 steps:

1. Create a new Flask-RESTful resource class
2. Create an endpoint method including the swagger docs decorator.
3. Add the new resource to the API at the bottom of the file.
