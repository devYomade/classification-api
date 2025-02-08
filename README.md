# Number Classification API

A simple API built with Go that classifies numbers based on mathematical properties and provides a fun fact.

## 🚀 Features
- Determines if a number is **prime** or **perfect**.
- Identifies properties like **Armstrong number**, **odd/even**, etc.
- Returns the **sum of digits**.
- Provides a **fun fact** about the number.
- Returns JSON responses with appropriate HTTP status codes.
- CORS-enabled for cross-origin requests.
- Fast response times (<500ms).

## 🛠 Tech Stack
- **Language:** Go (Golang)
- **Frameworks:** Standard Go HTTP library
- **Database:** None (All calculations are performed in-memory)
- **Deployment:** AWS EC2
- **Security:** CORS-enabled

## 📌 Installation & Setup
### 1️⃣ Prerequisites
Ensure you have the following installed:
- [Go](https://go.dev/dl/) (latest stable version)
- Git
- An AWS EC2 instance (for deployment)

### 2️⃣ Clone the Repository
```sh
git clone https://github.com/your-username/number-classifier.git
cd number-classifier
```

### 3️⃣ Install Dependencies
```sh
go mod tidy
```

### 4️⃣ Run the API Locally
```sh
go run main.go
```
The API should now be running on `http://localhost:8000`.

## 🌍 Deployment on AWS EC2
### 1️⃣ Set Up AWS Security Group Rules
- Open **port 8000** in your EC2 **security group**.
- Allow **inbound traffic** from `0.0.0.0/0` (public access) or restrict to your IP.

### 2️⃣ Upload the Project to EC2
```sh
scp -i your-key.pem -r number-classifier ec2-user@your-ec2-ip:/home/ec2-user/
```

### 3️⃣ SSH into EC2 and Run the API
```sh
ssh -i your-key.pem ec2-user@your-ec2-ip
cd number-classifier
go run main.go &
```
Use `screen` or `tmux` to keep it running in the background.

### 4️⃣ Test the Public API
```sh
curl "http://your-ec2-public-ip:8000/api/classify-number?number=371"
```

## 📌 API Usage
### **GET /api/classify-number?number=371**
#### ✅ **Response (200 OK)**
```json
{
    "number": 371,
    "is_prime": false,
    "is_perfect": false,
    "properties": ["armstrong", "odd"],
    "digit_sum": 11,
    "fun_fact": "371 is an Armstrong number because 3^3 + 7^3 + 1^3 = 371"
}
```
#### ❌ **Error Response (400 Bad Request)**
```json
{
    "number": "alphabet",
    "error": true
}
```

## 📜 Code Structure
```
number-classifier/
│── main.go       # Entry point of the API
│── handlers.go   # API handlers and business logic
│── utils.go      # Helper functions for number classification
│── go.mod        # Go module file
│── README.md     # Documentation
```

## 🛠 Running the API as a Background Service
To keep the API running after logout, use `systemd`:
1. Create a service file:
```sh
sudo nano /etc/systemd/system/number-api.service
```
2. Add the following content:
```ini
[Unit]
Description=Number Classification API
After=network.target

[Service]
ExecStart=/usr/local/go/bin/go run /home/ec2-user/number-classifier/main.go
WorkingDirectory=/home/ec2-user/number-classifier
Restart=always
User=ec2-user

[Install]
WantedBy=multi-user.target
```
3. Start and enable the service:
```sh
sudo systemctl daemon-reload
sudo systemctl start number-api
sudo systemctl enable number-api
```
4. Check service status:
```sh
sudo systemctl status number-api
```

## 🛡 Security Considerations
- Restrict API access to trusted IPs in AWS security group settings.
- Use a reverse proxy like **Nginx** for better scalability.
- Implement rate limiting to prevent abuse.

## 📜 License
This project is open-source and available under the MIT License.

---

**Author:** [Your Name]  
🚀 Happy coding!
