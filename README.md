# Stripe-with-go

Stripe Payment Integration API
A simple Go-based REST API for creating Stripe checkout sessions for payment processing.

🚀 Features
Create Stripe checkout sessions

Support for different pricing plans

User UUID tracking

Session expiration (30 minutes)

JSON-based API

Environment-based configuration

📋 Prerequisites
Go 1.19 or higher

Stripe account and API keys

Basic understanding of REST APIs

🛠️ Installation
1. Clone the Repository
bash
git clone <your-repository-url>
cd <repository-directory>
2. Install Dependencies
bash
go mod init <module-name>
go get github.com/stripe/stripe-go/v84
go get github.com/joho/godotenv
go mod tidy
3. Set Up Environment Variables
Create a .env file in the root directory:

env
STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key_here
⚠️ Important: Never commit your .env file to version control. Add it to .gitignore:

gitignore
.env
*.env
.env.local
4. Get Your Stripe API Keys
Sign up for a Stripe account at stripe.com

Navigate to the Stripe Dashboard

Copy your test secret key

Add it to your .env file

🚀 Running the Server
Development Mode
bash
go run main.go
The server will start on http://localhost:8080

Build and Run
bash
go build -o stripe-api main.go
./stripe-api
📖 API Documentation
Base URL
text
http://localhost:8080
Endpoints
1. Create Checkout Session
URL: /create-session

Method: POST

Content-Type: application/json

Request Body
json
{
    "user_uuid": "2bab7bc8-52cf-4da8-99c5-5a4cad8eabed",
    "price": "1000",
    "plan_name": "lms"
}
Parameters
Parameter	Type	Required	Description
user_uuid	string	Yes	Unique identifier for the user
price	string	Yes	Price in dollars (e.g., "1000" = $10.00)
plan_name	string	Yes	Name of the plan/subscription
Example Request using cURL
bash
curl -X POST http://localhost:8080/create-session \
  -H "Content-Type: application/json" \
  -d '{
    "user_uuid": "2bab7bc8-52cf-4da8-99c5-5a4cad8eabed",
    "price": "1000",
    "plan_name": "lms"
  }'
Example Response
json
{
    "status": 1,
    "message": "Checkout session created successfully",
    "status_code": 200,
    "session_url": {
        "user_uuid": "2bab7bc8-52cf-4da8-99c5-5a4cad8eabed",
        "session_url": "https://checkout.stripe.com/c/pay/cs_test_...",
        "session_id": "cs_test_b1tQRbtwDkrYIrGaPHwAnsVX98N9EWX6pXAKlgAWjRIZVYoOKfBhSF8KK9",
        "plan_name": "lms",
        "price": "1000",
        "created_at": "2025-12-19T20:22:53Z"
    }
}
2. Health Check
URL: /health

Method: GET

Example Response
json
{
    "status": "ok",
    "time": "2024-01-15T10:30:00Z"
}
🔧 Configuration
Environment Variables
Variable	Required	Default	Description
STRIPE_SECRET_KEY	Yes	-	Your Stripe secret key
PORT	No	8080	Server port
Payment Configuration
Currency: USD (hardcoded)

Session Expiry: 30 minutes

Mode: Payment (one-time)

Success URL: http://127.0.0.1:5500/Stripe-Payment-Go/payment-success.html

Cancel URL: http://127.0.0.1:5500/Stripe-Payment-Go/payment-failed.html

🎯 Usage Examples
Using JavaScript (Fetch API)
javascript
async function createCheckoutSession() {
    const response = await fetch('http://localhost:8080/create-session', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            user_uuid: '2bab7bc8-52cf-4da8-99c5-5a4cad8eabed',
            price: '1000',
            plan_name: 'lms'
        })
    });
    
    const data = await response.json();
    console.log(data);
    
    // Redirect to Stripe checkout
    if (data.session_url && data.session_url.session_url) {
        window.location.href = data.session_url.session_url;
    }
}
Using Python
python
import requests
import json

url = "http://localhost:8080/create-session"
payload = {
    "user_uuid": "2bab7bc8-52cf-4da8-99c5-5a4cad8eabed",
    "price": "1000",
    "plan_name": "lms"
}

response = requests.post(url, json=payload)
data = response.json()
print(json.dumps(data, indent=2))
📁 Project Structure
text
.
├── main.go              # Main application file
├── go.mod              # Go module file
├── go.sum              # Go dependencies checksum
├── .env               # Environment variables (create this)
├── .gitignore         # Git ignore file
└── README.md          # This file
🔍 Testing
Test with cURL
bash
# Test create session endpoint
curl -X POST http://localhost:8080/create-session \
  -H "Content-Type: application/json" \
  -d '{
    "user_uuid": "2bab7bc8-52cf-4da8-99c5-5a4cad8eabed",
    "price": "1000",
    "plan_name": "lms"
  }'

# Test health endpoint
curl http://localhost:8080/health
⚠️ Error Handling
The API returns appropriate HTTP status codes:

200: Success

400: Bad Request (missing or invalid parameters)

405: Method Not Allowed

500: Internal Server Error

🔒 Security Considerations
Use HTTPS in production: Never run this API over HTTP in production

Validate input: Always validate user input on the client side

Rate limiting: Implement rate limiting for production use

CORS: Configure CORS appropriately for your frontend

Key rotation: Regularly rotate your Stripe API keys

📝 Notes
The price should be provided as a string (e.g., "1000" for $10.00)

Session URLs expire after 30 minutes

All timestamps are in UTC format (RFC3339)

This uses Stripe test mode - use live keys for production

🆘 Troubleshooting
Common Issues
"STRIPE_SECRET_KEY environment variable is not set"

Ensure .env file exists in the root directory

Check that the variable name is exactly STRIPE_SECRET_KEY

"invalid price format"

Ensure price is a valid number string (e.g., "1000", "49.99")

"only POST method is allowed"

Ensure you're sending a POST request to the endpoint

"user_uuid, price, and plan_name are required"

Ensure all three fields are present in the request body
\
