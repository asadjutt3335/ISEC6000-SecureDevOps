
Task 1: Set Up Initial Infrastructure
Task1.1: Create a Kubernetes Cluster
 Log in to Google Cloud Console:
•	We go to the Google Cloud Console.
•	We sign in or create an account to setup google cloud console
Create a New Project:
•	We Click on the project drop-down in the top-left corner.
•	We Select "New Project" and give it a meaningful name.
Enable Kubernetes Engine API:
•	We navigate to "APIs & Services" → "Library".
•	We will Search for "Kubernetes Engine API" and click on "Enable".

 
Figure 1 Kubernetes Engine


 
Figure 2 API

Kubernetes Cluster Creation:
First, we need to install kubectl to create a cluster and it is also required to manage a cluster. I used this command for the installation of kubectl. 
Command: sudo apt-get install kubectl
 
Figure 3 Kubectl Installation
•	Now it is installed successfully after that if we install the Kubectl we can make a cluster. Now the next process will be defined as the creation of a cluster. The below command was used to create a cluster
Command: gcloud container clusters create –machine-type n1-standard-2 –num-nodes 2 –zone us-central1-a –cluster-version latest e-commerce


Wait for the Cluster to be Provisioned:
•	It might take a few minutes to do this. When it is going to be available, I’ll be able to look at my cluster in the list of those available in the Ku-bernetes Engine panel.
Task1.2: Configure Kubectl
To configure Kubectl to manage your Kubernetes cluster from your local environment, follow these steps
      Step 1: Install Kubectl
If you haven't installed kubectl yet, you can do so using the following methods:
•	On Windows:
Command: choco install Kubectl
Step 2: Connect and authenticate kubectl to your GKE Cluster
After you have Kubectl installed you will want to configure it to connect to your Google Ku-bernetes Engine (GKE).
o	Authenticate to Google Cloud:
o	Ensure you are authenticated with the correct Google Cloud account using:
Command: gcloud auth login


o	Get Credentials for Your Cluster:
o	Use gcloud to get the credentials for your Kubernetes cluster. This command sets up kubectl to use your GKE cluster.
Command: gcloud container clusters get-credentials e-commerce --zone us-central1-a
Step 3: Verify kubectl Configuration
After configuring kubectl, verify that it’s correctly set up:
•	Check Cluster Access:
o	Run the following command to see if kubectl can access your cluster:
Command: kubectl get nodes
o	You should see a list of nodes in your cluster if everything is configured correct-ly.
 
Figure 6 Cluster Access
Task1.3: GitHub Repository Setup:
Step 1: Create a New GitHub Repository
•	Log in to GitHub:
o	Go to GitHub and log in with your account.
•	Create a New Repository:
o	Click on the + icon in the upper-right corner of the GitHub interface and select New repository.
•	Set Repository Details:
o	Repository Name: ISEC6000-SecureDevOps.
o	Description: “This repo contains files for the ISEC6000 Secure DevOps as-signment”
o	Public: Make sure that the visibility of the repository is public.

o	Initialize with README: The tick mark should be placed there to have the system automatically create a README file for the repository at the project creation. 
•	Create Repository:
o	Click the Create repository button.

 
Figure 8 Repository Created
	
Step 2: Add a Proper README File
•	Edit the README.md:
o	Once the repository is created, you'll see the README.md file in the reposito-ry.
o	Click on the README.md file to open it, then click the pencil icon to edit.
•	Customize the README.md:
o	Include the following information:
	Project Title: "ISEC6000 Secure DevOps Project".
	Description: Provide a brief overview of the project and its goals.
	Installation Instructions: Add steps for setting up the project if neces-sary.
	Usage: Describe how to use the project.
	License: Mention any licensing information if applicable.
	Authors: Include name and any collaborators.
•	Commit Changes:
o	After editing, scroll down and enter a commit message (e.g., "Added project overview and setup instructions").
o	Click Commit changes to save your changes.
Task 2: Microservices Architecture and Deployment
Step 1: Explore the Core Saleor Projects
1.1. Saleor API
•	Explore the API repository: I visit the Saleor API repository at https://github.com/saleor/saleor
•	Understand its functionalities: The Saleor API handles the backend logic and data-base interactions for the e-commerce platform.
1.2. Saleor Dashboard
•	Explore the Dashboard repository: I visit the Saleor Dashboard repository at https://github.com/saleor/saleor-dashboard
•	Understand the Dashboard: The Dashboard provides the admin interface for manag-ing the Saleor e-commerce platform.
1.3. Saleor Platform
•	Explore the Platform repository: I Visit the Saleor Platform repository at https://github.com/saleor/saleor-platform.
•	Understand its role: This repository contains the Docker Compose configuration for deploying the Saleor API, Dashboard, and related services. It uses Git submodules to reference the other Saleor repositories.
Step 2: Fork the Saleor Platform Repository
2.1	Create a Personal GitHub Account:
o	We go to GitHub and sign in with our account details.
 
Figure 9 GitHub
2.2	Fork the Saleor Platform Repository:
o	We will visit the https://github.com/saleor/saleor-platform repository.
o	Click on the "Fork" button in the top-right corner to fork the repository to your GitHub account.
Step 3: Deploy the Saleor Stack
3.1	Clone Your Forked Repository:
o	Clone the forked Saleor Platform repository to your local machine:
code
git clone https://github.com/asadjutt3335/saleor-platform

 
Figure 10 Git Clone

3.2	Follow the Deployment Instructions:
o	Navigate to the cloned repository directory:
code
cd saleor-platform
o	We will Follow the step-by-step guidelines provided in the Saleor Platform re-pository’s README file to deploy the stack.
Step 4: Customize the Docker Compose Configuration
      4.1 Assign Port 9002 to the Saleor Dashboard:
o	Open the docker-compose.yml file in the Saleor Platform repository.
o	Locate the section that defines the Saleor Dashboard service.
o	Modify the port configuration to map port 9002 on your host to the appropri-ate port inside the container:
services:
  api:
    image: ghcr.io/saleor/saleor:3.20
    ports:
      - 8000:8000
    restart: unless-stopped
    networks:
      - saleor-backend-tier
    stdin_open: true
    tty: true
    depends_on:
      - db
      - redis
      - jaeger
    volumes:
      # shared volume between worker and api for media
      - saleor-media:/app/media
    env_file:
      - common.env
      - backend.env
    environment:
      - JAEGER_AGENT_HOST=jaeger
      - DASHBOARD_URL=http://192.168.1.5:9002/
      - ALLOWED_HOSTS=192.168.1.5,api

  dashboard:
    image: ghcr.io/saleor/saleor-dashboard:latest
    ports:
      - 9002:80
    restart: unless-stopped
    networks:
      - saleor-backend-tier

  db:
    image: library/postgres:15-alpine
    ports:
      - 5432:5432
    restart: unless-stopped
    networks:
      - saleor-backend-tier
    volumes:
      - saleor-db:/var/lib/postgresql/data
      - ./replica_user.sql:/docker-entrypoint-initdb.d/replica_user.sql
    environment:
      - POSTGRES_USER=saleor
      - POSTGRES_PASSWORD=saleor

  redis:
    image: library/redis:7.0-alpine
    ports:
      - 6379:6379
    restart: unless-stopped
    networks:
      - saleor-backend-tier
    volumes:
      - saleor-redis:/data

  worker:
    image: ghcr.io/saleor/saleor:3.20
    command: celery -A saleor --app=saleor.celeryconf:app worker --loglevel=info -B
    restart: unless-stopped
    networks:
      - saleor-backend-tier
    env_file:
      - common.env
      - backend.env
    depends_on:
      - redis
      - mailpit
    volumes:
      # shared volume between worker and api for media
      - saleor-media:/app/media

  jaeger:
    image: jaegertracing/all-in-one
    ports:
      - "5775:5775/udp"
      - "6831:6831/udp"
      - "6832:6832/udp"
      - "5778:5778"
      - "16686:16686"
      - "14268:14268"
      - "9411:9411"
    restart: unless-stopped
    networks:
      - saleor-backend-tier
    volumes:
      - type: tmpfs
        target: /tmp

  mailpit:
    image: axllent/mailpit
    ports:
      - 1025:1025 # smtp server
      - 8025:8025 # web ui. Visit http://192.168.1.5:8025/ to check emails
    restart: unless-stopped
    networks:
      - saleor-backend-tier

volumes:
  saleor-db:
    driver: local
  saleor-redis:
    driver: local
  saleor-media:

networks:
  saleor-backend-tier:
    driver: bridge

4.2	Initiate the Stack:
To start the process first we have to install docker and need to configure it ac-cording to our requirements 
 
Figure 11 Docker

Code
•	Build the application:
docker compose build
•	Apply Django migrations:

C:\program Files\Docker\resources\bin\docker.exe compose run --rm api python3 manage.py migrate






Figure 12  Docker Compose Run1 
 
Figure 15 Docker Compose Run4
•	Populate the database with example data and create the admin user:

docker compose run --rm api python3 manage.py populatedb –createsuperuser








•	Run the application:
       docker compose up




4.3	Verify Saleor Dashboard Accessibility:
      Open your web browser and navigate to http://192.168.1.5:9002 to access the Saleor Dashboard.
 
Figure 19 Dashboard1

 
Figure 20 Dashboard2




 
Figure 21 Dashboard3

Step 5: Version Control
5.1	Commit Your Changes:
Add your changes to the Docker Compose configuration:
                             code
git add .
git commit -m "Assigned port 9002 to Saleor Dashboard and customized Docker Compose file"
5.2	Tag the Commit:
Tag the commit with isec6000-assignment1:
code
git tag isec6000-assignment1
5.3	Push Changes to GitHub:
             Push your changes and tags to your GitHub repository:
code
git push origin main --tags

Task 3: Implementing Security Measures
To ensure the security of your microservices application, we'll implement the following securi-ty measures:
3.1. Container Security:
•	Running Containers as Non-Root Users:
o	By default, many Docker containers run as the root user, which can pose a sig-nificant security risk. We'll modify the Docker Compose file to run containers as non-root users.
o	Steps:
	Update your Dockerfile for each service to include a non-root user. For example, for the api service, you might add:
Dockerfile
code
Inside your Dockerfile
FROM ghcr.io/saleor/saleor:3.20
RUN addgroup --system saleor && adduser --system --ingroup saleor saleor
USER saleor
	If using an image where you can't modify the Dockerfile, add the fol-lowing in the Docker Compose file:
yaml
code
services:
  api:
    user: "1000:1000"  # Replace with non-root user/group ID
•	Setting Resource Limits:
o	Resource limits help prevent a container from consuming too many resources, which could potentially lead to a denial of service.
o	Steps:
	Update your Docker Compose file to include resource limits for each service. For example:
yaml
code
services:
  api:
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: "512M"
        reservations:
          cpus: "0.5"
          memory: "256M"

•	Using Secure Base Images:
o	Ensure that you use official, trusted images and keep them up to date with the latest security patches.
o	Steps:
	Regularly update the FROM line in your Dockerfile to use the latest stable version of the base image:
Dockerfile
code
FROM ghcr.io/saleor/saleor:3.20 
3.2. Vulnerability Scanning with Trivy:
•	Trivy is an open-source vulnerability scanner for container images, filesystem, and Git repositories.
•	Steps:
3.2.1	Install Trivy:
     Scoop is a command-line installer for Windows, and it's a convenient way to install Trivy.
3.2.2	Install Scoop:
o	Open PowerShell as Administrator.
o	Run the following command to install Scoop:
powershell
code
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
iwr -useb get.scoop.sh | iex
3.2.3	Install Trivy using Scoop:
 After Scoop is installed, you can install Trivy with this command:
powershell
code
scoop install trivy

            3.2.4 Run a scan on your container images:
Code
trivy image ghcr.io/saleor/saleor:3.20
 
Figure 24 Trivy Analysis2
 
Figure 25  Trivy Analysis3
 
Figure 26  Trivy Analysis4

Task 4: Architecture Visualization
Here's a detailed overview of the connection details for each component in your architecture diagram:
Creating the Diagram
1.	Open your chosen diagramming tool.
2.	Add and label each component based on the services and volumes listed above.
3.	Connect the components using lines to show how they interact.
4.	Annotate the diagram with:
o	Ports: Show which ports are exposed by which services.
o	Security Measures: Add notes on network policies or access controls.

Diagram Components
4.1. Saleor API
•	Shape: Rectangle
•	Label: “Saleor API”
•	Port: 8000
•	Function: Handles API requests, business logic, and user authentication.
•	Connections:
o	To PostgreSQL:
	Connection Type: Data flow
	Arrow: From Saleor API to PostgreSQL
	Purpose: Sends data requests and queries to the database for data re-trieval and storage.
o	To Redis:
	Connection Type: Data flow
	Arrow: From Saleor API to Redis
	Purpose: Uses Redis for caching data and session management.
o	To Saleor Dashboard:
	Connection Type: Data flow
	Arrow: From Saleor API to Saleor Dashboard
	Purpose: Provides data and functionality to the admin interface.
4.2. PostgreSQL
•	Shape: Cylinder
•	Label: “PostgreSQL”
•	Port: 5432
•	Function: Data storage for application data.
•	Connections:
o	From Saleor API:
	Connection Type: Data requests
	Arrow: From Saleor API to PostgreSQL
	Purpose: Receives queries and data requests from Saleor API to per-form CRUD operations.
4.3. Redis
•	Shape: Rectangle
•	Label: “Redis”
•	Port: 6379
•	Function: Caching layer.
•	Connections:
o	From Saleor API:
	Connection Type: Data flow
	Arrow: From Saleor API to Redis
	Purpose: Receives caching requests and session data from Saleor API.
4.4. Saleor Dashboard
•	Shape: Rectangle
•	Label: “Saleor Dashboard”
•	Port: 9002
•	Function: Admin interface for managing the platform.
•	Connections:
o	From Saleor API:
	Connection Type: Data flow
	Arrow: From Saleor Dashboard to Saleor API
	Purpose: Sends requests to the API to fetch or update data, and to perform administrative tasks.
4.5. Volumes
•	Shape: Cloud (or another suitable shape)
•	Label: “Volumes”
•	Function: Persistent storage used by PostgreSQL and potentially Redis.
•	Connections:
o	To PostgreSQL:
	Connection Type: Storage connection
	Arrow: From Volumes to PostgreSQL
	Purpose: Provides persistent storage for PostgreSQL data.
o	To Redis (if used):
	Connection Type: Storage connection (optional)
	Arrow: From Volumes to Redis
	Purpose: Provides storage for Redis data if needed.
4.6. Internal Network
•	Shape: Dashed Rectangle or Cloud
•	Label: “Internal Network”
•	Function: Represents the private network connecting services.
•	Connections:
o	Encapsulates: Saleor API, PostgreSQL, Redis, Saleor Dashboard
	Purpose: Shows that these services are within the same internal net-work and communicate through it.
4.7. External Network
•	Shape: Dashed Rectangle or Cloud
•	Label: “External Network”
•	Function: Represents the public-facing network through which users access the plat-form.
•	Connections:
o	To Saleor API:
	Arrow: From External Network to Saleor API
	Purpose: Users from the external network access Saleor API for API requests.
o	To Saleor Dashboard:
	Arrow: From External Network to Saleor Dashboard
	Purpose: Users from the external network access the admin dashboard for management tasks.
4.8. External Users/Admins
•	Shape: Rectangle
•	Label: “External Users/Admins”
•	Function: Users accessing the Saleor Dashboard and API from the internet.
•	Connections:
o	To Saleor API:
	Arrow: From External Users/Admins to Saleor API
	Purpose: External users send API requests to Saleor API.
o	To Saleor Dashboard:
	Arrow: From External Users/Admins to Saleor Dashboard
	Purpose: External admins access the Saleor Dashboard for administra-tive tasks.
4.9. Summary of Connections
•	Saleor API: Connects to PostgreSQL, Redis, and Saleor Dashboard.
•	PostgreSQL: Receives data requests from Saleor API.
•	Redis: Connects to Saleor API for caching.
•	Saleor Dashboard: Connects to Saleor API for data.
•	Volumes: Connects to PostgreSQL and optionally Redis for persistent storage.
•	Internal Network: Includes Saleor API, PostgreSQL, Redis, and Saleor Dashboard.
•	External Network: Connects to Saleor API and Saleor Dashboard, representing pub-lic access.
•	External Users/Admins: Connect to Saleor API and Saleor Dashboard from the ex-ternal network.



