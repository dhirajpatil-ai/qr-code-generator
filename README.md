# QR Code Generator using FastAPI

A simple web-based QR Code Generator built using **Python, FastAPI, Jinja2, and the `qrcode` library**.

The application allows users to enter any text or website URL and immediately generate a downloadable QR code image.

The project also demonstrates how to:

* Build a Python web application using FastAPI
* Containerize the application using Docker
* Push the Docker image to Docker Hub
* Deploy the application on Kubernetes
* Run and access the application locally using Minikube

---

## Features

* Generate QR codes from text
* Generate QR codes from website URLs
* Download the generated QR code as a PNG image
* Simple web interface
* FastAPI REST endpoints
* Health-check endpoint
* Docker support
* Kubernetes deployment support
* Runs completely on localhost

---

## Project Architecture

```text
User
 │
 │ Text / URL
 ▼
FastAPI Web Application
 │
 ▼
qrcode Python Library
 │
 ▼
QR Code PNG
 │
 └── Download
```

For Kubernetes:

```text
Browser
   │
   ▼
NodePort Service
   │
   ▼
Kubernetes Deployment
   │
   ├── Pod 1
   │
   └── Pod 2
         │
         ▼
     FastAPI App
         │
         ▼
   QR Code Generator
```

---

## Technologies Used

| Technology | Purpose                  |
| ---------- | ------------------------ |
| Python     | Application development  |
| FastAPI    | Web framework            |
| Uvicorn    | ASGI server              |
| qrcode     | QR code generation       |
| Pillow     | Image processing         |
| Jinja2     | HTML templates           |
| HTML/CSS   | Web interface            |
| Docker     | Containerization         |
| Docker Hub | Container image registry |
| Kubernetes | Container orchestration  |
| Minikube   | Local Kubernetes cluster |

---

## Project Structure

```text
qr-code-generator/
│
├── app/
│   ├── __init__.py
│   ├── main.py
│   │
│   ├── templates/
│   │   └── index.html
│   │
│   └── static/
│       └── style.css
│
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── .gitignore
├── deployment.yaml
└── README.md
```

---

# 1. Clone or Create the Project

Create the project directory:

```powershell
mkdir qr-code-generator
cd qr-code-generator
```

Create the required directories:

```powershell
mkdir app
mkdir app\templates
mkdir app\static
```

---

# 2. Create Python Virtual Environment

Create a virtual environment:

```powershell
python -m venv .venv
```

Activate it:

```powershell
.venv\Scripts\activate
```

Upgrade pip:

```powershell
python -m pip install --upgrade pip
```

---

# 3. Install Dependencies

The project uses the following dependencies:

```text
fastapi
uvicorn[standard]
qrcode[pil]
jinja2
python-multipart
```

Install them using:

```powershell
pip install -r requirements.txt
```

---

# 4. Run the Application Locally

Start the FastAPI application:

```powershell
uvicorn app.main:app --reload
```

The application will start at:

```text
http://127.0.0.1:8000
```

Open the URL in your browser:

```text
http://localhost:8000
```

---

# 5. Generate a QR Code

Enter any text or URL in the input field.

For example:

```text
https://www.google.com
```

Click:

```text
Generate & Download QR Code
```

The application generates a QR code and downloads it as:

```text
qr_code.png
```

You can also enter normal text:

```text
Hello from FastAPI QR Code Generator
```

---

# 6. FastAPI Endpoints

## Home Page

```text
GET /
```

Displays the QR Code Generator web interface.

---

## Generate QR Code

```text
POST /generate
```

Receives the user's text or URL and generates a PNG QR code.

---

## Health Check

```text
GET /health
```

Response:

```json
{
    "status": "healthy"
}
```

This endpoint is also used by Kubernetes for readiness and liveness checks.

---

# 7. FastAPI Swagger Documentation

FastAPI automatically provides API documentation.

Open:

```text
http://localhost:8000/docs
```

You can test the available API endpoints from the Swagger UI.

---

# 8. Docker

## Build Docker Image

Make sure Docker Desktop is running.

Build the image:

```powershell
docker build -t qr-code-generator:1.0 .
```

Check the image:

```powershell
docker images
```

---

## Run Docker Container

Run the application:

```powershell
docker run -d -p 8000:8000 --name qr-code-app qr-code-generator:1.0
```

Check the running container:

```powershell
docker ps
```

Open:

```text
http://localhost:8000
```

The application is now running inside a Docker container.

---

## Docker Logs

To view application logs:

```powershell
docker logs qr-code-app
```

---

## Stop Container

```powershell
docker stop qr-code-app
```

Remove the container:

```powershell
docker rm qr-code-app
```

---

# 9. Docker Hub

## Login

Login to Docker Hub:

```powershell
docker login
```

---

## Tag the Image

Replace `yourusername` with your Docker Hub username.

```powershell
docker tag qr-code-generator:1.0 yourusername/qr-code-generator:1.0
```

Create the latest tag:

```powershell
docker tag qr-code-generator:1.0 yourusername/qr-code-generator:latest
```

---

## Push Image to Docker Hub

```powershell
docker push yourusername/qr-code-generator:1.0
```

Also push the latest tag:

```powershell
docker push yourusername/qr-code-generator:latest
```

The image will then be available from Docker Hub as:

```text
yourusername/qr-code-generator:latest
```

---

# 10. Kubernetes Deployment

The project contains:

```text
deployment.yaml
```

This YAML file creates:

1. Kubernetes Deployment
2. Two application Pods
3. Kubernetes NodePort Service

The deployment uses:

```yaml
replicas: 2
```

Therefore, Kubernetes runs two replicas of the application.

---

# 11. Start Minikube

Start your local Kubernetes cluster:

```powershell
minikube start --driver=docker
```

Check Minikube:

```powershell
minikube status
```

Check Kubernetes nodes:

```powershell
kubectl get nodes
```

---

# 12. Deploy Application to Kubernetes

Before deployment, update the Docker image in `deployment.yaml`.

For example:

```yaml
image: yourusername/qr-code-generator:latest
```

Then apply the configuration:

```powershell
kubectl apply -f deployment.yaml
```

Expected output:

```text
deployment.apps/qr-code-generator created
service/qr-code-generator-service created
```

---

# 13. Check Kubernetes Deployment

Check the Deployment:

```powershell
kubectl get deployments
```

Check Pods:

```powershell
kubectl get pods
```

Expected:

```text
NAME                                  READY   STATUS
qr-code-generator-xxxxxxxxxx-xxxxx   1/1     Running
qr-code-generator-xxxxxxxxxx-xxxxx   1/1     Running
```

Check the Service:

```powershell
kubectl get services
```

---

# 14. Access Application Through Minikube

Run:

```powershell
minikube service qr-code-generator-service
```

Minikube will open the application in your browser.

Alternatively, get the URL:

```powershell
minikube service qr-code-generator-service --url
```

Open the displayed URL in your browser.

---

# 15. Test the Kubernetes Application

Enter sample text:

```text
Hello from Kubernetes!
```

Click:

```text
Generate & Download QR Code
```

The QR code should be downloaded as:

```text
qr_code.png
```

You can scan the QR code using a mobile phone.

Also test a website URL:

```text
https://www.python.org
```

---

# 16. Kubernetes Health Check

The application provides:

```text
GET /health
```

Kubernetes uses this endpoint for:

### Readiness Probe

Determines whether the application is ready to receive traffic.

### Liveness Probe

Determines whether the application is still running correctly.

Example:

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8000

livenessProbe:
  httpGet:
    path: /health
    port: 8000
```

---

# 17. Useful Kubernetes Commands

Check all resources:

```powershell
kubectl get all
```

Check Pods:

```powershell
kubectl get pods
```

Check Pod details:

```powershell
kubectl describe pod <pod-name>
```

View application logs:

```powershell
kubectl logs <pod-name>
```

Check Deployment:

```powershell
kubectl get deployment
```

Check Service:

```powershell
kubectl get service
```

Delete the deployment:

```powershell
kubectl delete -f deployment.yaml
```

---

# 18. Complete Workflow

The complete application workflow is:

```text
Python Application
       │
       ▼
FastAPI
       │
       ▼
Test on localhost
       │
       ▼
Docker Image
       │
       ▼
Docker Container
       │
       ▼
Test on localhost
       │
       ▼
Docker Hub
       │
       ▼
Kubernetes Deployment
       │
       ▼
Kubernetes Pods
       │
       ▼
Kubernetes Service
       │
       ▼
Minikube
       │
       ▼
Local Browser
       │
       ▼
Generate QR Code
       │
       ▼
Download PNG
```

---

# 19. Example

Input:

```text
https://www.google.com
```

Application:

```text
FastAPI
   ↓
qrcode library
   ↓
PNG image
```

Output:

```text
qr_code.png
```

The QR code can then be scanned to open the provided website.

---

# 20. Future Improvements

Possible improvements for the project:

* QR code preview before downloading
* Custom QR code colors
* Custom QR code size
* Logo inside QR code
* Download as JPG/SVG
* QR code history
* Generate Wi-Fi QR codes
* Generate contact/vCard QR codes
* User authentication
* Database integration
* CI/CD using GitHub Actions
* Kubernetes Ingress
* Horizontal Pod Autoscaling
* Cloud deployment on AWS/Azure

---

## Author

**Dhiraj Patil**

AI/ML Engineer | Python | FastAPI | Docker | Kubernetes | Generative AI

---

## License

This project is intended for learning and demonstration purposes.
