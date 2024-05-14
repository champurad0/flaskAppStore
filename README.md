# flaskAppStore

# Flask App 

## Will create a simple REST API

1. Start by creating a virtual enviornment for the project.

// the the "-m" allows the the module to run at the cmd 
// the "venv" is calling the virtual env library 
// the "env" is the name given to the enviornment 

`python3 -m venv env`

2. You can then verify which packages are install in the envionrment

`pip list` 

Package Version
------- -------
pip     24.0

this will display all installed libraries.

3. Now you can begin installing packages with pip as needed

`pip install flask`

Package      Version
------------ -------
blinker      1.8.2
click        8.1.7
Flask        3.0.3
itsdangerous 2.2.0
Jinja2       3.1.4
MarkupSafe   2.1.5
pip          24.0
Werkzeug     3.0.3

Upon completion all dependencies can be saved using 

`pip freeze > requirements.txt`

# Misc notes while working on App
---

Building a simple data store and returning it can verify the routes and db are in working order

example 
`
// this is the db or data we want returned 
stores = [
    {
        "name": "My Store",
        "items": [
            {
                "name": "Chair", 
                "price": 15.99
            }
        ]
    }
]

// the `.get` method allows the data to be requested by visiting the url listed in the route
@app.get("/store/")
def get_stores():
    return {"stores": stores}
`

this is the data returned in JSON format once you visit /store

`
{"stores":[{"items":[{"name":"Chair","price":15.99}],"name":"My Store"}]}
`

// the `.post` method will allow data to be sent from users to the API

`
@app.post("/store/")
def create_store():
// this will get the collect JSON sent into the API
    request_data = request.get_json()

// this will parse through the JSON 
    new_store = {"name": request_data["name"], "items": []}

// this will add the data to the dictionary
    stores.append(new_store)
    return new_store, 201
`

---

// this route will allow items to be added to a particular store 
`
@app.post("/store/<string:name>/item")
def create_item(name):
    request_data = request.get_json()
    for store in stores:
        if store["name"] == name:
            new_item = {"name": request_data["name"], "price": request_data["price"]}
            store["items"].append(new_item)
            return new_item, 201
    return {"message": "Store not found"}, 404

`

### Running Flask app within docker

Create a docker image with our flask code.
// this is the base of our docker image

`touch Dockerfile`

// inside the docker file we add 
`
FROM python:3.10
EXPOSE 5000
WORKDIR /app
RUN pip install flask 
COPY . .
CMD ["flask", "run", "--host", "0.0.0.0"]
`

// then we would run docker build to create the image and cache all the layers

`docker build -t {{dockerImageName}} .`

// docker build will have to be rerun after all changes made to the app
// since layers are stored in cache each sub build will be faster.


