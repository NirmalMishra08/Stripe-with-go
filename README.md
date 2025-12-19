# Stripe Payment Integration API
A simple Go-based REST API for creating Stripe checkout sessions for payment processing.

---

## 🚀 Features
* **Create Stripe checkout sessions** with a single POST request.
* **Support for different pricing plans** (dynamic price and plan naming).
* **User UUID tracking** to link Stripe sessions to your internal users.
* **Session expiration** (30 minutes) to ensure security and data freshness.
* **JSON-based API** for easy integration with modern frontends.
* **Environment-based configuration** using `.env` files.

---

## 📋 Prerequisites
* **Go 1.19** or higher
* **Stripe Account** and API keys ([Get them here](https://dashboard.stripe.com/apikeys))
* **Basic understanding** of REST APIs

---

## 🛠️ Installation

### 1. Clone the Repository
```bash
git clone <your-repository-url>
cd <repository-directory>
2. Install Dependencies
Bash

go mod init <module-name>
go get [github.com/stripe/stripe-go/v84](https://github.com/stripe/stripe-go/v84)
go get [github.com/joho/godotenv](https://github.com/joho/godotenv)
go mod tidy
3. Set Up Environment Variables
Create a .env file in the root directory:

Code snippet

STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key_here
PORT=8080
⚠️ Important: Never commit your .env file to version control.

🚀 Running the Server
Development Mode
Bash

go run main.go
The server will start on http://localhost:8080

Build and Run
Bash

go build -o stripe-api main.go
./stripe-api
📖 API Documentation
1. Create Checkout Session
URL: /create-session | Method: POST

Request Body:

JSON

{
    "user_uuid": "2bab7bc8-52cf-4da8-99c5-5a4cad8eabed",
    "price": "1000",
    "plan_name": "lms"
}
Success Response:

JSON

{
    "status": 1,
    "message": "Checkout session created successfully",
    "status_code": 200,
    "session_url": {
        "user_uuid": "2bab7bc8...",
        "session_url": "[https://checkout.stripe.com/c/pay/cs_test](https://checkout.stripe.com/c/pay/cs_test)...",
        "session_id": "cs_test...",
        "plan_name": "lms",
        "price": "1000",
        "created_at": "2025-12-19T20:22:53Z"
    }
}
2. Health Check
URL: /health | Method: GET

🔧 Configuration & Defaults
Currency: USD

Session Expiry: 30 minutes

Mode: Payment (one-time)

Success URL: http://127.0.0.1:5500/Stripe-Payment-Go/payment-success.html

Cancel URL: http://127.0.0.1:5500/Stripe-Payment-Go/payment-failed.html

📁 Project Structure
Plaintext

.
├── main.go              # Main application logic
├── .env               # Environment variables (private)
├── .gitignore         # Prevents .env from being uploaded
├── go.mod              # Go module definition
└── README.md          # Project documentation
🔒 Security Considerations
Production: Always use HTTPS.

Secrets: Use secret management tools in production instead of plain .env files where possible.

Webhooks: For production, implement Stripe Webhooks to confirm payments server-side.

🆘 Troubleshooting
"missing STRIPE_SECRET_KEY": Ensure your .env file is in the root directory.

"invalid price format": Ensure price is a string representing cents (e.g., "1000" for $10.00).
