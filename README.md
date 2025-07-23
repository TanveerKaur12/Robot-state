# 🤖 Robot State Tracking API (Serverless on AWS)

This project is a serverless API that tracks and stores the **state, location, and battery level** of a robot using **AWS Lambda**, **API Gateway**, and **DynamoDB**.  
It also includes a simple HTML frontend (optional) to submit data.

---

## 📦 Features

- ✅ Serverless architecture using AWS
- 🧠 Unique `robotid` for each state entry
- 📤 REST API to POST robot state
- 💾 Stores data in DynamoDB
- 🌐 Optional frontend (HTML form)
- 🗣️ Voice & visual enhancements (planned)

---

## 🔧 Technologies Used

- **AWS Lambda** – backend logic
- **API Gateway (HTTP API)** – API endpoint
- **DynamoDB** – NoSQL database to store robot states
- **UUID** – to generate unique robot IDs
- **CORS** – configured to allow web frontend calls
- **HTML** – basic frontend form

---

## 🛠️ Lambda Function: `lambda_function.py`

```python
import json
import boto3
import uuid

def lambda_handler(event, context):
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('robotstate')
    
    body = json.loads(event['body'])
    
    robotid = str(uuid.uuid4())
    state = body.get('state')
    location = body.get('location')
    battery = body.get('battery')

    table.put_item(
        Item={
            'robotid': robotid,
            'state': state,
            'location': location,
            'battery': battery
        }
    )

    return {
        'statusCode': 200,
        'headers': {
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps({
            'message': 'Robot state saved successfully',
            'robotid': robotid
        })
    }
