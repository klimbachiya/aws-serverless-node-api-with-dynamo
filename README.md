# Serverless REST API with DynamoDB and offline support

This example demonstrates how to run a service locally, using the [serverless-offline](https://github.com/dherault/serverless-offline) plugin. It
provides a REST API to manage Todos stored in a DynamoDB. A local DynamoDB instance is provided by the [serverless-dynamodb](https://www.npmjs.com/package/serverless-dynamodb) plugin. This needs dynamoDB installed locally.

## Deployment

```bash
npm install -g serverless
npm install
serverless deploy
```

## Local Development

### Install DynamoDB Locally

Download dynamoDB locally per [dynamoDB local installation steps](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html). Start dynamoDB locally using below step:

```bash
java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb -inMemory -port <portNumer>
```

### Setup

```bash
npm install
serverless plugin install -n serverless-dynamodb
serverless plugin install -n serverless-offline
```

One should see 2 plugins namely serverless-dynamodb & serverless-offline at the end of serverless.yml file.

### Run service offline

```bash
serverless offline start
```

### Usage

You can create, retrieve, update, or delete todos with the following commands:

#### Create a Todo

```bash
curl -X POST -H "Content-Type:application/json" http://localhost:3000/todos --data '{ "text": "Learn Serverless" }'
```

Example Result:
```bash
{"text":"Learn Serverless","id":"ee6490d0-aa11e6-9ede-afdfa051af86","createdAt":1479138570824,"checked":false,"updatedAt":1479138570824}%
```

#### List all Todos

```bash
curl -H "Content-Type:application/json" http://localhost:3000/todos
```

Example output:
```bash
[{"text":"Deploy my first service","id":"ac90feaa11e6-9ede-afdfa051af86","checked":true,"updatedAt":1479139961304},{"text":"Learn Serverless","id":"206793aa11e6-9ede-afdfa051af86","createdAt":1479139943241,"checked":false,"updatedAt":1479139943241}]%
```

#### Get one Todo

```bash
# Replace the <id> part with a real id from your todos table
curl -H "Content-Type:application/json" http://localhost:3000/todos/<id>
```

Example Result:
```bash
{"text":"Learn Serverless","id":"ee6490d0-aa11e6-9ede-afdfa051af86","createdAt":1479138570824,"checked":false,"updatedAt":1479138570824}%
```

#### Update a Todo

```bash
# Replace the <id> part with a real id from your todos table
curl -X PUT -H "Content-Type:application/json" http://localhost:3000/todos/<id> --data '{ "text": "Learn Serverless", "checked": true }'
```

Example Result:
```bash
{"text":"Learn Serverless","id":"ee6490d0-aa11e6-9ede-afdfa051af86","createdAt":1479138570824,"checked":true,"updatedAt":1479138570824}%
```

#### Delete a Todo

```bash
# Replace the <id> part with a real id from your todos table
curl -X DELETE -H "Content-Type:application/json" http://localhost:3000/todos/<id>
```

No output