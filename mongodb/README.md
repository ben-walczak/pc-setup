# MongoDB

MongoDB is a NoSQL database that stores unstructured data. It utilizes dynamic schemas making it very flexible. Data is mostly stored in a JSON like format, so reading and writing data is often very human friendly. The biggest con that MongoDB cannot do is relational mapping or joining multiple data sets together.

## MongoDB vs MySQL
| MongoDB         | MySql     |
|--------------|-----------|
| It displays data as a collection of JSON documents. | It displays data in tables and rows. |
| It uses object querying.	| It is a structured query language. |
| Less risk of attack due to its design. | The risk of SQL injection attacks is there. |
| It is an ideal option if you have structured and/or unstructured data with the potential for quick growth. | It is an ideal option if you have structured data and need a traditional relational database. |
| High availability, scalability, replication, and sharding are inbuilt. | No efficient replication and sharding are available. |
| MongoDB uses Collection, Document, Field, Embedded Document, Linking, etc. | MySQL uses Table, Row, Column, Joins, etc. |

- [codersera](https://codersera.com/blog/mongodb-vs-mysql/)

## Docker Setup
You can start a throwaway MongoDB container:
```bash
docker run -d -p 27017:27017 --name example-mongo mongo:latest
```

This will provide a live running server using the official image from [dockerhub](https://hub.docker.com/_/mongo/).

Often, we'll want to specify the data volume, username, and password, which looks like the following:
```bash
docker run -d 
    -p 27017:27017 
    --name example-mongo 
    -v mongo-data:/data/db 
    -e MONGODB_INITDB_ROOT_USERNAME=example-user 
    -e MONGODB_INITDB_ROOT_PASSWORD_FILE=/run/secrets/mongo-root-pw 
    mongo:latest
```

To specify a specific mongo config use the `--config` tag:
```bash
docker run -d 
    --name example-mongo 
    -v mongo-data:/data/db 
    -v ./mongo.conf:/etc/mongo/mongo.conf
    mongo:latest --config /etc/mongo/mongo.conf
```
- [Source](https://www.howtogeek.com/devops/how-to-run-mongodb-in-a-docker-container/)

## Kubernetes Setup
Looking at the Docker configuration, we can see that mongodb is utilizing volumes to store its data. Since databases need data to persist after its container has shut down, we would want to utilize persistent volumes. To have a persistent volume, we should also specify how much storage is needed, via a persistent volume claim. Its good practice to have username and password credentials saved as a secret in kubernetes. These secrets can then be specified in other kubernetes manifests without referencing what the username or password is. For the sake of complexity, we will define our secret in the manifest file base64 encoded. However, a better practice is to import the secrets from vault or environment variables.

This provides us with the following manifest files:
- mongodb-deployment.yaml
- mongodb-pv.yaml
- mongodb-pvc.yaml
- mongodb-secrets.yaml

To create/apply these changes to minikube, simply `cd` to this directory and execute the following:
```bash
kubectl apply -f .
```

To access mongodb from outside the mongo pod, specify mongo's secrets via the `deployment.yaml` and port number to access mongodb via python like so:
```python
from pymongo import MongoClient

# Load config from a .env file:
load_dotenv()
MONGODB_URI = os.environ['MONGODB_URI']

# Connect to your MongoDB cluster:
client = MongoClient(MONGODB_URI)
```
- [MongoDB Python Access](https://www.mongodb.com/developer/languages/python/python-quickstart-crud/)


Might be helpful to either save the mongodb uri or rather mongo configuration file to hide usernames/passwords. Need to look into this further.

- [Source](https://devopscube.com/deploy-mongodb-kubernetes/)

## Final notes on Docker/Kubernetes
The end goal is to deploy everything via kubernetes manifest files, but that's pretty difficult. I've said before that linux is probably the deepest rabbit hole of a technology, but I think kubernetes takes second in this race pretty easy. That being said, to understand some of the complexities of it is to deploy it via Docker. If we can learn how to deploy mongodb via docker, then we can learn how most containerized applications via Docker can be translated to kubernetes manifests. Now I'm not saying all technologies are the same in this regard, but mongodb is complex enough to help teach us how to take other containerized applications and turn them into kubernetes manifest files.
