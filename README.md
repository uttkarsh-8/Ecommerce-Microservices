# Microservices Project

This project consists of multiple microservices managed with Maven and Docker. Follow the steps below to set up and run the project.

## Prerequisites

- Java JDK (version 17 or higher)
- Maven
- Docker and Docker Compose
- Postman (for API testing)

## Getting Started

1. Clone this repository to your local machine.

2. Navigate to the project root directory.

3. Build all Maven projects

   This will build the following services:
   - config-server
   - customer
   - discovery
   - gateway
   - notification
   - order
   - payment
   - product

4. Start the services using Docker Compose:

   ```
   docker-compose up -d
   ```

## Keycloak Configuration

After starting the services, you need to configure Keycloak for authentication:

1. Access Keycloak at `http://localhost:9098`

2. Log in with the following credentials:
   - Username: admin
   - Password: admin

3. Create a new realm:
   - Click on the dropdown in the top-left corner
   - Select "Create realm"
   - Name the realm "micro-services"
   - Click "Create"

4. Create a new client:
   - Go to "Clients" in the left sidebar
   - Click "Create client"
   - Follow these steps:
     
     a. General Settings:
     - Client type: OpenID Connect
     - Client ID: micro-services-api
     - Name: micro-services-api
     - Description: micro-services-api
     - Always display in UI: On
     - Click "Next"

     b. Capability config:
     - Client authentication: On
     - Authorization: On
     - Authentication flow: Check only "Service accounts roles"
     - Click "Save"

   - Leave Root URL and Home URL empty

5. Configure OpenID Endpoint:
   - In the realm settings, click on "Endpoints"
   - Click on "OpenID Endpoint Configuration"
   - Find and save the `token_endpoint` URL (e.g., `http://localhost:9098/realms/micro-services/protocol/openid-connect/token`)

6. Get Client Secret:
   - Go back to "Clients" and click on "micro-services-api"
   - Go to the "Credentials" tab
   - Copy the client secret

## Starting the Services

Start the services in the following order:

1. Config Server
2. Discovery Server
3. All other services

You can start them individually or use Docker Compose to start them all at once.

## Testing with Postman

To test your APIs with authentication:

1. Open Postman
2. Create a new request
3. Go to the "Authorization" tab
4. Choose "OAuth 2.0" as the authentication type
5. Set the following details:
   - Grant Type: Client Credentials
   - Access Token URL: [The `token_endpoint` URL you saved earlier]
   - Client ID: micro-services-api
   - Client Secret: [The client secret you copied]
6. Click "Get New Access Token"
7. Use the received token in your requests


## Sample Postman Collection

{
	"info": {
		"_postman_id": "2af9e602-0dc7-4408-80d6-5d34bc0308bf",
		"name": "Microservices",
		"schema": "https://schema.getpostman.com/json/collection/v2.0.0/collection.json",
		"_exporter_id": "29333777",
		"_collection_link": "https://speeding-firefly-295720.postman.co/workspace/trial-1~b3470255-fb7a-41ec-a085-8121eece3338/collection/29333777-2af9e602-0dc7-4408-80d6-5d34bc0308bf?action=share&source=collection_link&creator=29333777"
	},
	"item": [
		{
			"name": "Customers",
			"request": {
				"auth": {
					"type": "oauth2",
					"oauth2": {
						"clientSecret": "Z4PjwX8394Xh8whlXUyCN9QW1Monbfxg",
						"accessTokenUrl": "http://localhost:9098/realms/micro-services/protocol/openid-connect/token",
						"clientId": "micro-services-api",
						"grant_type": "client_credentials",
						"addTokenTo": "header"
					}
				},
				"method": "POST",
				"header": [],
				"body": {
					"mode": "raw",
					"raw": "{\n  \"firstname\": \"ronalldo\",\n  \"lastname\": \"criss\",\n  \"email\": \"1@124.com\",\n  \"address\": {\n    \"street\": \"Street name\",\n    \"houseNumber\": \"123\",\n    \"zipCode\": \"50001\"\n  }\n}",
					"options": {
						"raw": {
							"language": "json"
						}
					}
				},
				"url": "http://localhost:8222/api/v1/customers"
			},
			"response": []
		},
		{
			"name": "Orders",
			"request": {
				"auth": {
					"type": "oauth2",
					"oauth2": {
						"clientSecret": "Z4PjwX8394Xh8whlXUyCN9QW1Monbfxg",
						"accessTokenUrl": "http://localhost:9098/realms/micro-services/protocol/openid-connect/token",
						"clientId": "micro-services-api",
						"grant_type": "client_credentials",
						"addTokenTo": "header"
					}
				},
				"method": "POST",
				"header": [],
				"body": {
					"mode": "raw",
					"raw": "{\n  \"reference\": \"MS-21231393\",\n  \"amount\": 900,\n  \"paymentMethod\": \"PAYPAL\",\n  \"customerId\": \"66a923e29577233d63c09d5a\",\n  \"products\": [\n    {\n      \"productId\": 1,\n      \"quantity\": 1\n    },\n    {\n      \"productId\": 251,\n      \"quantity\": 1\n    }\n  ]\n}",
					"options": {
						"raw": {
							"language": "json"
						}
					}
				},
				"url": "http://localhost:8222/api/v1/orders"
			},
			"response": []
		},
		{
			"name": "Products",
			"request": {
				"auth": {
					"type": "oauth2",
					"oauth2": {
						"clientSecret": "Z4PjwX8394Xh8whlXUyCN9QW1Monbfxg",
						"accessTokenUrl": "http://localhost:9098/realms/micro-services/protocol/openid-connect/token",
						"clientId": "micro-services-api",
						"grant_type": "client_credentials",
						"addTokenTo": "header"
					}
				},
				"method": "GET",
				"header": [],
				"url": "http://localhost:8222/api/v1/products"
			},
			"response": []
		}
	]
}
