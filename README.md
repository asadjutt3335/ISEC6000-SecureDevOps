# Report 

## By
**Muhammad Assad Sattar**

**Student ID:** 21263211

**MSc Computing (Cybersecurity)**

**Department of Computing Sciences, School of EECMS**  
**Curtin University, Bentley Campus**

**August 30, 2024**


---

## Table of Contents
1. [Task 1: Set Up Initial Infrastructure](#task-1-set-up-initial-infrastructure)
    - [Task 1.1: Create a Kubernetes Cluster](#task-11-create-a-kubernetes-cluster)
    - [Task 1.2: Configure Kubectl](#task-12-configure-kubectl)
    - [Task 1.3: GitHub Repository Setup](#task-13-github-repository-setup)
2. [Task 2: Microservices Architecture and Deployment](#task-2-microservices-architecture-and-deployment)
3. [Task 3: Implementing Security Measures](#task-3-implementing-security-measures)
4. [Task 4: Architecture Visualization](#task-4-architecture-visualization)
5. [References](#references)

---

# ISEC6000 Secure DevOps Project

## Task 1: Set Up Initial Infrastructure

### Task 1.1: Create a Kubernetes Cluster

1. **Log in to Google Cloud Console:**
   - We go to the Google Cloud Console.
   - We sign in or create an account to set up Google Cloud Console.

2. **Create a New Project:**
   - We click on the project drop-down in the top-left corner.
   - We select "New Project" and give it a meaningful name.

3. **Enable Kubernetes Engine API:**
   - We navigate to "APIs & Services" → "Library".
   - We search for "Kubernetes Engine API" and click on "Enable".

   ![image](https://github.com/user-attachments/assets/89552a3d-ae7b-40ba-818b-7b157448e6c1)

   *Figure 1 Kubernetes Engine*


   ![image](https://github.com/user-attachments/assets/84a2f3dd-b8ed-4a6d-a5c2-8a61c9ae6992)

   *Figure 2 API*

4. **Kubernetes Cluster Creation:**
   - First, we need to install kubectl to create and manage a cluster. The command used for installing kubectl is:
     ```bash
     sudo apt-get install kubectl
     ```
     ![image](https://github.com/user-attachments/assets/a67e745a-c9b3-4c7d-9250-8f0a30a48dda)

     *Figure 3 Kubectl Installation*

   - After installation, we can create a cluster using the following command:
     ```bash
     gcloud container clusters create --machine-type n1-standard-2 --num-nodes 2 --zone us-central1-a --cluster-version latest e-commerce
     ```
     - Wait for the cluster to be provisioned. Once available, it will appear in the Kubernetes Engine panel.

### Task 1.2: Configure Kubectl

1. **Step 1: Install Kubectl**
   - If not installed, use the following command on Windows:
     ```bash
     choco install kubectl
     ```

2. **Step 2: Connect and authenticate kubectl to your GKE Cluster**
   - **Authenticate to Google Cloud:**
     ```bash
     gcloud auth login
     ```
     ![image](https://github.com/user-attachments/assets/e62768cf-0a46-45f0-a0c2-14c4f3b818a1)

   - **Get Credentials for Your Cluster:**
     ```bash
     gcloud container clusters get-credentials e-commerce --zone us-central1-a
     ```

3. **Step 3: Verify kubectl Configuration**
   - **Check Cluster Access:**
     ```bash
     kubectl get nodes
     ```
     - You should see a list of nodes in your cluster if everything is configured correctly.

     ![image](https://github.com/user-attachments/assets/1de1f2bd-155f-4829-9d09-0f7105e71055)

     *Figure 6 Cluster Access*

### Task 1.3: GitHub Repository Setup

1. **Step 1: Create a New GitHub Repository**
   - **Log in to GitHub:**
     - Go to GitHub and log in with your account.
   - **Create a New Repository:**
     - Click on the + icon in the upper-right corner and select New repository.
       ![image](https://github.com/user-attachments/assets/462961f0-feff-4a6f-a6ad-0a07e3543db5)

   - **Set Repository Details:**
     - **Repository Name:** `ISEC6000-SecureDevOps`
     - **Description:** "This repo contains files for the ISEC6000 Secure DevOps assignment"
     - **Visibility:** Public
     - **Initialize with README:** Make sure this is checked.
   - **Create Repository:**
     - Click the "Create repository" button.

     ![image](https://github.com/user-attachments/assets/786a7a7d-d706-49ad-88f3-0372f2fb100a)

     *Figure 8 Repository Created*

2. **Step 2: Add a Proper README File**
   - **Edit the README.md:**
     - Open the README.md file and click the pencil icon to edit.
   - **Customize the README.md:**
     - Include the following information:
       - **Project Title:** "ISEC6000 Secure DevOps Project"
       - **Description:** Provide a brief overview of the project and its goals.
       - **Installation Instructions:** Add steps for setting up the project if necessary.
       - **Usage:** Describe how to use the project.
       - **License:** Mention any licensing information if applicable.
       - **Authors:** Include name and any collaborators.
   - **Commit Changes:**
     - Enter a commit message (e.g., "Added project overview and setup instructions") and click "Commit changes".

## Task 2: Microservices Architecture and Deployment

### Step 1: Explore the Core Saleor Projects

1. **Saleor API**
   - **Explore the API repository:** Visit the [Saleor API repository](https://github.com/saleor/saleor).
   - **Understand its functionalities:** The Saleor API handles the backend logic and database interactions for the e-commerce platform.

2. **Saleor Dashboard**
   - **Explore the Dashboard repository:** Visit the [Saleor Dashboard repository](https://github.com/saleor/saleor-dashboard).
   - **Understand the Dashboard:** The Dashboard provides the admin interface for managing the Saleor e-commerce platform.

3. **Saleor Platform**
   - **Explore the Platform repository:** Visit the [Saleor Platform repository](https://github.com/saleor/saleor-platform).
   - **Understand its role:** This repository contains the Docker Compose configuration for deploying the Saleor API, Dashboard, and related services.

### Step 2: Fork the Saleor Platform Repository

1. **Create a Personal GitHub Account:**
   - Sign in to GitHub with your account details.

   ![image](https://github.com/user-attachments/assets/0fd84905-7db0-498b-8d2d-ef87d2545bee)

   *Figure 9 GitHub*

2. **Fork the Saleor Platform Repository:**
   - Visit the [Saleor Platform repository](https://github.com/saleor/saleor-platform).
   - Click the "Fork" button in the top-right corner to fork the repository to your GitHub account.

### Step 3: Deploy the Saleor Stack

1. **Clone Your Forked Repository:**
   - Clone the forked Saleor Platform repository to your local machine:
     ```bash
     git clone https://github.com/asadjutt3335/saleor-platform
     ```
     ![image](https://github.com/user-attachments/assets/42bcd58e-baa4-45da-adf7-1b177001d4dd)
     *Figure 10 Git Clone*

2. **Follow the Deployment Instructions:**
   - Navigate to the cloned repository directory:
     ```bash
     cd saleor-platform
     ```
   - Follow the step-by-step guidelines provided in the Saleor Platform repository’s README file to deploy the stack.

### Step 4: Customize the Docker Compose Configuration

1. **Assign Port 9002 to the Saleor Dashboard:**
   - Open the `docker-compose.yml` file in the Saleor Platform repository.
   - Modify the port configuration to map port 9002 on your host to the appropriate port inside the container:
     ```yaml
     services:
       dashboard:
         ports:
           - 9002:80
     ```

2. **Initiate the Stack:**
   - **Build the application:**
     ```bash
     docker compose build
     ```
   - **Apply Django migrations:**
     ```bash
     docker compose run --rm api python3 manage.py migrate
     ```
   - **Populate the database with example data and create the admin user:**
     ```bash
     docker compose run --rm api python3 manage.py populatedb --createsuperuser
     ```
   - **Run the application:**
     ```bash
     docker compose up
     ```

3. **Verify Saleor Dashboard Accessibility:**
   - Open your web browser and navigate to `http://192.168.1.5:9002` to access the Saleor Dashboard.

     ![image](https://github.com/user-attachments/assets/36b62a12-ea95-4708-833e-eee6b758721f)

     *Figure 19 Dashboard1*

    ![image](https://github.com/user-attachments/assets/0ba009d4-7037-40b8-ab1d-3cdebe0b533d)

     *Figure 20 Dashboard2*

     ![image](https://github.com/user-attachments/assets/f20d9b08-3a44-4132-80dd-f1c93dd863e7)

     *Figure 21 Dashboard3*

### Step 5: Version Control

1. **Commit Your Changes:**
   - Add your changes to the Docker Compose configuration:
     ```bash
     git add .
     git commit -m "Assigned port 9002 to Saleor Dashboard and customized Docker Compose file"
     ```

2. **Tag the Commit:**
   - Tag the commit with `isec6000-assignment1`:
     ```bash
     git tag isec6000-assignment1
     ```

3. **Push Changes to GitHub:**
   - Push your changes and tags to your GitHub repository:
     ```bash
     git push origin main --tags
     ```

# Task 3: Implementing Security Measures

To ensure the security of your microservices application, we'll implement the following security measures:

## 3.1. Container Security

### Running Containers as Non-Root Users

By default, many Docker containers run as the root user, which can pose a significant security risk. We'll modify the Docker Compose file to run containers as non-root users.

#### Steps:

1. Update your Dockerfile for each service to include a non-root user. For example, for the `api` service, you might add:

    ```Dockerfile
    # Inside your Dockerfile
    FROM ghcr.io/saleor/saleor:3.20
    RUN addgroup --system saleor && adduser --system --ingroup saleor saleor
    USER saleor
    ```

2. If using an image where you can't modify the Dockerfile, add the following in the Docker Compose file:

    ```yaml
    services:
      api:
        user: "1000:1000"  # Replace with non-root user/group ID
    ```

### Setting Resource Limits

Resource limits help prevent a container from consuming too many resources, which could potentially lead to a denial of service.

#### Steps:

1. Update your Docker Compose file to include resource limits for each service. For example:

    ```yaml
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
    ```

### Using Secure Base Images

Ensure that you use official, trusted images and keep them up to date with the latest security patches.

#### Steps:

1. Regularly update the `FROM` line in your Dockerfile to use the latest stable version of the base image:

    ```Dockerfile
    FROM ghcr.io/saleor/saleor:3.20
    ```

## 3.2. Vulnerability Scanning with Trivy

Trivy is an open-source vulnerability scanner for container images, filesystem, and Git repositories.

### Steps:

#### 3.2.1 Install Trivy

Scoop is a command-line installer for Windows, and it's a convenient way to install Trivy.

#### 3.2.2 Install Scoop

1. Open PowerShell as Administrator.
2. Run the following command to install Scoop:

    ```powershell
    Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
    iwr -useb get.scoop.sh | iex
    ```

#### 3.2.3 Install Trivy using Scoop

After Scoop is installed, you can install Trivy with this command:

```powershell
scoop install trivy
```
![image](https://github.com/user-attachments/assets/9c90f4e7-2a15-4cad-b30f-51ff32fcc2ca)

#### 3.2.4 Run a scan on your container images:
trivy image ghcr.io/saleor/saleor:3.20
![image](https://github.com/user-attachments/assets/00fa6148-4069-46e3-8640-8617c76117bf)

![image](https://github.com/user-attachments/assets/8cfe90fd-2bf8-4d72-a219-f9528d0950b9)

![image](https://github.com/user-attachments/assets/70bbbc1c-f9f5-4d3d-9761-b5861a31339e)

![image](https://github.com/user-attachments/assets/b7d73b87-84cc-40d9-97a5-4b1eaa7eb21c)


## Architecture Diagram Connection Details

### Creating the Diagram
1. Open your chosen diagramming tool.
2. Add and label each component based on the services and volumes listed above.
3. Connect the components using lines to show how they interact.
4. Annotate the diagram with:
   - **Ports**: Show which ports are exposed by which services.
   - **Security Measures**: Add notes on network policies or access controls.

### Diagram Components

#### 4.1. Saleor API
- **Shape**: Rectangle
- **Label**: “Saleor API”
- **Port**: 8000
- **Function**: Handles API requests, business logic, and user authentication.
- **Connections**:
  - **To PostgreSQL**:
    - **Connection Type**: Data flow
    - **Arrow**: From Saleor API to PostgreSQL
    - **Purpose**: Sends data requests and queries to the database for data retrieval and storage.
  - **To Redis**:
    - **Connection Type**: Data flow
    - **Arrow**: From Saleor API to Redis
    - **Purpose**: Uses Redis for caching data and session management.
  - **To Saleor Dashboard**:
    - **Connection Type**: Data flow
    - **Arrow**: From Saleor API to Saleor Dashboard
    - **Purpose**: Provides data and functionality to the admin interface.

#### 4.2. PostgreSQL
- **Shape**: Cylinder
- **Label**: “PostgreSQL”
- **Port**: 5432
- **Function**: Data storage for application data.
- **Connections**:
  - **From Saleor API**:
    - **Connection Type**: Data requests
    - **Arrow**: From Saleor API to PostgreSQL
    - **Purpose**: Receives queries and data requests from Saleor API to perform CRUD operations.

#### 4.3. Redis
- **Shape**: Rectangle
- **Label**: “Redis”
- **Port**: 6379
- **Function**: Caching layer.
- **Connections**:
  - **From Saleor API**:
    - **Connection Type**: Data flow
    - **Arrow**: From Saleor API to Redis
    - **Purpose**: Receives caching requests and session data from Saleor API.

#### 4.4. Saleor Dashboard
- **Shape**: Rectangle
- **Label**: “Saleor Dashboard”
- **Port**: 9002
- **Function**: Admin interface for managing the platform.
- **Connections**:
  - **From Saleor API**:
    - **Connection Type**: Data flow
    - **Arrow**: From Saleor Dashboard to Saleor API
    - **Purpose**: Sends requests to the API to fetch or update data, and to perform administrative tasks.

#### 4.5. Volumes
- **Shape**: Cloud (or another suitable shape)
- **Label**: “Volumes”
- **Function**: Persistent storage used by PostgreSQL and potentially Redis.
- **Connections**:
  - **To PostgreSQL**:
    - **Connection Type**: Storage connection
    - **Arrow**: From Volumes to PostgreSQL
    - **Purpose**: Provides persistent storage for PostgreSQL data.
  - **To Redis (if used)**:
    - **Connection Type**: Storage connection (optional)
    - **Arrow**: From Volumes to Redis
    - **Purpose**: Provides storage for Redis data if needed.

#### 4.6. Internal Network
- **Shape**: Dashed Rectangle or Cloud
- **Label**: “Internal Network”
- **Function**: Represents the private network connecting services.
- **Connections**:
  - **Encapsulates**: Saleor API, PostgreSQL, Redis, Saleor Dashboard
    - **Purpose**: Shows that these services are within the same internal network and communicate through it.

#### 4.7. External Network
- **Shape**: Dashed Rectangle or Cloud
- **Label**: “External Network”
- **Function**: Represents the public-facing network through which users access the platform.
- **Connections**:
  - **To Saleor API**:
    - **Arrow**: From External Network to Saleor API
    - **Purpose**: Users from the external network access Saleor API for API requests.
  - **To Saleor Dashboard**:
    - **Arrow**: From External Network to Saleor Dashboard
    - **Purpose**: Users from the external network access the admin dashboard for management tasks.

#### 4.8. External Users/Admins
- **Shape**: Rectangle
- **Label**: “External Users/Admins”
- **Function**: Users accessing the Saleor Dashboard and API from the internet.
- **Connections**:
  - **To Saleor API**:
    - **Arrow**: From External Users/Admins to Saleor API
    - **Purpose**: External users send API requests to Saleor API.
  - **To Saleor Dashboard**:
    - **Arrow**: From External Users/Admins to Saleor Dashboard
    - **Purpose**: External admins access the Saleor Dashboard for administrative tasks.

#### 4.9. Summary of Connections
- **Saleor API**: Connects to PostgreSQL, Redis, and Saleor Dashboard.
- **PostgreSQL**: Receives data requests from Saleor API.
- **Redis**: Connects to Saleor API for caching.
- **Saleor Dashboard**: Connects to Saleor API for data.
- **Volumes**: Connects to PostgreSQL and optionally Redis for persistent storage.
- **Internal Network**: Includes Saleor API, PostgreSQL, Redis, and Saleor Dashboard.
- **External Network**: Connects to Saleor API and Saleor Dashboard, representing public access.
- **External Users/Admins**: Connect to Saleor API and Saleor Dashboard from the external network.
![image](https://github.com/user-attachments/assets/33184796-3c59-434e-84f9-e08c5109e5d6)
