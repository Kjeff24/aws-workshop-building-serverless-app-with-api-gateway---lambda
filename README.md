<h1>AWS WORKSHOP - BUILDING SERVERLESS APP WITH API GATEWAY & LAMBDA</h1>

# USAGE
- Get further understanding about workshop at https://catalog.workshops.aws/serverless-patterns/en-US/business-scenario
- Login to AWS Cognito for the `ID_TOKEN`
```
aws cognito-idp initiate-auth --auth-flow USER_PASSWORD_AUTH --client-id <client_id> --auth-parameters USERNAME=<username>,PASSWORD=<password> --query 'AuthenticationResult.IdToken' --output text
```
- To format JSON in Ubuntu, you can use the jq command-line tool. It’s a powerful and easy-to-use JSON processor that can pretty-print JSON data.
```
sudo apt update
sudo apt install jq
```

## USER SERVICE

export USER_SERVICE_ENDPOINT=<your_order_service_endpoint>
export ID_TOKEN=<your_user_id_token

### ADD USER
```
curl --location --request POST "$USER_SERVICE_ENDPOINT/users" \
--data-raw '{
  "resource": "/users",
  "path": "/users",
  "httpMethod": "POST",
  "headers": null,
  "multiValueHeaders": null,
  "queryStringParameters": null,
  "multiValueQueryStringParameters": null,
  "pathParameters": null,
  "stageVariables": null,
  "requestContext": {
      "requestId": "be946131-30c4-4396-9c29-f25f8caa91dc"
  },
  "body": "{\"name\":\"Kojo Fees\"}",
  "isBase64Encoded": false
}' \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

### GET USERS
```
curl --location --request GET "$USER_SERVICE_ENDPOINT/users" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

### GET A USER
```
curl --location --request GET "$USER_SERVICE_ENDPOINT/users/<user_id>" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

## UPDATE USER
```
curl --location --request PUT "$USER_SERVICE_ENDPOINT/users/<user_id>" \
--data-raw '{
  "resource": "/users",
  "path": "/users",
  "httpMethod": "POST",
  "headers": null,
  "multiValueHeaders": null,
  "queryStringParameters": null,
  "multiValueQueryStringParameters": null,
  "pathParameters": null,
  "stageVariables": null,
  "requestContext": {
      "requestId": "be946131-30c4-4396-9c29-f25f8caa91dc"
  },
  "body": "{\"name\":\"Kojo Fees\"}",
  "isBase64Encoded": false
}' \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

### DELETE USER
```
curl --location --request DELETE "$USER_SERVICE_ENDPOINT/users/<user_id>" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

## ORDER SERVICE

export ORDER_SERVICE_ENDPOINT=<your_order_service_endpoint>/orders
export ID_TOKEN=<your_user_id_token>

### ADD ORDER

```
curl --location --request POST "$ORDER_SERVICE_ENDPOINT" \
--data-raw '{
    "restaurantId": 1,
    "orderId": "<order_id>",
    "orderItems": [
        {
            "id": 1,
            "name": "Spaghetti",
            "price": 9.99,
            "quantity": 1
        },
        {
            "id": 2,
            "name": "Pizza - SMALL",
            "price": 4.99,
            "quantity": 2
        }
    ],
    "totalAmount": 19.97,
    "status": "PLACED"
}' \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

### GET ORDERS

```
curl --location --request GET "$ORDER_SERVICE_ENDPOINT" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json" | jq .
```

### GET AN ORDER

```
curl --location --request GET "$ORDER_SERVICE_ENDPOINT/<order_id>" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json" | jq .
```

### EDIT ORDER
Ensure you include the orderTime after the order was placed
```
curl --location --request PUT "$ORDER_SERVICE_ENDPOINT/<order_id>" \
--data-raw '{
    "restaurantId": 1,
    "orderItems": [
        {
            "id": 1,
            "name": "Paparazi",
            "price": 9.99,
            "quantity": 1
        },
        {
            "id": 2,
            "name": "Pizza - SMALL",
            "price": 4.99,
            "quantity": 2
        }
    ],
    "totalAmount": 19.97,
    "status": "PLACED",
    "orderTime": <order_time>,
}' \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

### DELETE AN ORDER

```
curl --location --request DELETE "$ORDER_SERVICE_ENDPOINT/<order_id>" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"
```

## USER PROFILE SERVICE

export USER_PROFILE_SERVICE_ENDPOINT=<your_order_service_endpoint>  
export ID_TOKEN=<your_user_id_token>

### ADD ADDRESS

```
curl --location --request POST "$USER_PROFILE_SERVICE_ENDPOINT/address" \
--data-raw '{
        "line1": "123 Main St",
        "line2": "Apt 4B",
        "city": "Sample City",
        "stateProvince": "CA",
        "postal": "12345",
        "userId": "user-123"
    }' \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json" | jq .
```

### GET ADDRESSES

```
curl --location --request GET "$USER_PROFILE_SERVICE_ENDPOINT/address/" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json" | jq .
```

### EDIT ADDRESS

```
curl --location --request PUT "$USER_PROFILE_SERVICE_ENDPOINT/address/<address_id>" \
--data-raw '{
        "line1": "123 Main St",
        "line2": "Apt 4B",
        "city": "Sample City",
        "stateProvince": "CA",
        "postal": "7778",
        "userId": "user-123"
    }' \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json" | jq .
```

### DELETE ADDRESS

```
curl --location --request DELETE "$USER_PROFILE_SERVICE_ENDPOINT/address/<address_id>" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json" | jq .
```

### ADD FAVOURITE

```
curl --location --request POST "$USER_PROFILE_SERVICE_ENDPOINT/favorite" \
--data-raw '{"restaurantId": "f45445454nnD"}' \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

### LIST FAVOURITE

```
curl --location --request GET "$USER_PROFILE_SERVICE_ENDPOINT/favorite" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

### DELETE FAVOURITE

```
curl --location --request DELETE "$USER_PROFILE_SERVICE_ENDPOINT/favorite/<favorite_id>" \
--header "Authorization: $ID_TOKEN" \
--header "Content-Type: application/json"  | jq .
```

## ORDER STATUS SERVICE
