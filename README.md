# Travel Memory Application

A full-stack travel memory journal web app built using the **MERN stack**, deployed on **AWS EC2**, and load-balanced with custom domain management via **Cloudflare**.
---

## Repository

🔗 [GitHub: Travel Memory App](https://github.com/UnpredictablePrashant/TravelMemory)
---

## Features

- Node.js backend & React frontend deployment.
- Load balancing via AWS Application Load Balancer.
- Multi-instance scaling with EC2 & AMI snapshots.
- MongoDB Atlas database.
- Nginx reverse proxy for port management.
- Domain setup with Cloudflare (e.g. `tanujbhatia.site`).
---

## Tech Stack

- **Frontend**: React
- **Backend**: Node.js, Express
- **Database**: MongoDB Atlas
- **Cloud**: AWS EC2, Load Balancer, AMI, VPC
- **DNS**: Cloudflare
- **Web Server**: Nginx
---

### Prerequisites
1. Ensure the backend EC2 public IP is whitelisted in MongoDB Atlas.
2. All EC2 instances should be launched in the ap-south-1 (Mumbai) region.
3. Use AMI snapshots to easily replicate and scale frontend/backend instances.
4. Ensure that Port 80 (HTTP) is open in the EC2 Security Group to allow incoming web traffic.
---

## Quick Deployment Guide

### Backend Setup

1. Launch EC2 Instance (Linux) for backend setup.<br>
   Public IP -  3.7.68.34 <br>
   Private IP - 172.31.1.215 
   - ![image](https://github.com/user-attachments/assets/d3f2ffe8-029b-4bb1-91a4-5eaf577b60be)

3. Install Node.js and clone repo.
   ```bash
   # Download and install fnm:
   curl -o- https://fnm.vercel.app/install | bash
   
   # Download and install Node.js:
   fnm install 22
   
   # Verify the Node.js version:
   node -v # Should print "v22.14.0".
   
   # Verify npm version:
   npm -v # Should print "10.9.2".

   # Install git and clone the repo
   sudo yum install git -y
   git clone https://github.com/UnpredictablePrashant/TravelMemory
   ```
4. Navigate to backend folder and create .env file and install the dependencies of the application.
   ```bash
   # Go to backend directory
   cd ~/TravelMemory/backend

   # Create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements)
   MONGO_URI=your_mongo_uri
   PORT=3000

   # Install & Build the dependencies
   sudo npm install

   # Start the application
   sudo node index.js
   ```
5. Start the backend application at port 3000 and validate.<br>
   <img width="511" alt="image" src="https://github.com/user-attachments/assets/3e964302-bc01-4b36-9e89-5398ec45de4d" /><br>
   <img width="532" alt="image" src="https://github.com/user-attachments/assets/026f1b94-4cc4-4a2d-819b-2bfc3768df02" /><br>
    
6. Setup Nginx reverse proxy on port 80.
   ```bash
   sudo yum install nginx -y
   sudo systemctl enable nginx
   sudo nano /etc/nginx/nginx.conf
   ```
   Example nginx config.
   ```bash
   server {
    listen 80;
    server_name _;
    location / {
        proxy_pass http://localhost:3000;
     }
   }
   ```
   Restart & Reload nginx new config.
   ```bash
   sudo systemctl reload nginx
   sudo systemctl restart nginx
   sudo nginx -t #to validate nginx config file
   ```
   Start the nodejs application.
   ```bash
   node index.js
   ```
7. Now we can access the application at port 80 because of reverse proxy.<br>
   <img width="514" alt="image" src="https://github.com/user-attachments/assets/def014a6-099b-42ab-8064-3b572bb4c862" /><br>
   <img width="527" alt="image" src="https://github.com/user-attachments/assets/232d9e0e-2f04-406d-846b-abb3c3fa7b3f" />
---

### Frontend Setup

1. Launch separate EC2 instances (Linux) for frontend.<br>
   Public IP - 13.233.115.125 <br>
   Private IP - 172.31.2.209
   - ![image](https://github.com/user-attachments/assets/0cc199a7-7c77-4f90-8c97-e36da7a25242)


3. Install Node.js, clone the same repository.
   ```bash
   # Download and install fnm:
   curl -o- https://fnm.vercel.app/install | bash
   
   # Download and install Node.js:
   fnm install 22
   
   # Verify the Node.js version:
   node -v # Should print "v22.14.0".
   
   # Verify npm version:
   npm -v # Should print "10.9.2".

   # Install git and clone the repo
   sudo yum install git -y
   git clone https://github.com/UnpredictablePrashant/TravelMemory
   ```
4. Navigate to frontend folder and create .env file and install the dependencies of the application.
   ```bash
   # Go to frontend directory
   cd ~/TravelMemory/frontend

   # create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements): 
   REACT_APP_BACKEND_URL=http://backend_server_ip #eg. - http://3.110.217.207

   # install the dependencies
   npm install
   ```
5. Update urls.js with backend IP or domain.
   ```bash
   # Go to frontend/src directory
   cd ~/TravelMemory/frontend/src
   
   # Put the following content (remember to change it based on your requirements): 
   export const baseUrl = process.env.REACT_APP_BACKEND_URL #Calling the value from .env file directy using variable "REACT_APP_BACKEND_URL"
   ```
6. Start the frontend application at port 3000 and validate.
   ```bash
   npm run build
   npm start
   ```
  - ![image](https://github.com/user-attachments/assets/7fe55f9f-2a3c-429b-a0e7-ff82ae47393c)


7. Setup Nginx reverse proxy on port 80 for frontend just like backend.
   ```bash
   sudo yum install nginx -y
   sudo systemctl enable nginx
   sudo nano /etc/nginx/nginx.conf
   ```
   Example nginx config.
   ```bash
   server {
    listen 80;
    server_name _;
    location / {
        proxy_pass http://localhost:3000;
     }
   }
   ```
   Restart & Reload nginx new config.
   ```bash
   sudo systemctl reload nginx
   sudo systemctl restart nginx
   sudo nginx -t #to validate nginx config file
   ```
   Build and start the application
   ```bash
   npm run build
   npm start
   ```
8. Now we can access the application at port 80 because of reverse proxy.<br>
   ### add pic
---

### Load Balancing & Scaling

1. Create AMIs for both frontend and backend instances.<br>
   <img width="823" alt="image" src="https://github.com/user-attachments/assets/3a4d264f-5dce-4d2d-be95-cd697b66cb2d" /><br>
2. Launch backend & frontend instances using these AMIs.<br>
   <img width="957" alt="image" src="https://github.com/user-attachments/assets/d996d982-133a-4e66-9fb6-33a49d6dd20e" /><br>
3. For Load Balancing, create Target Groups for frontend and backend.<br>
   <img width="429" alt="image" src="https://github.com/user-attachments/assets/44f46082-5026-440b-a522-e64856734490" /><br>
   <img width="398" alt="image" src="https://github.com/user-attachments/assets/aea679c1-94a4-4841-959a-9c54061c7b8a" /><br>
4. Create Application Load Balancers and assign target groups.<br>
   NOTE : Make sure the Load balancer is mapped to multi AZs
   <img width="959" alt="image" src="https://github.com/user-attachments/assets/d737c3c2-d550-48b0-8821-13bf94450d4a" /><br>
   ![image](https://github.com/user-attachments/assets/b5a9b575-d518-423c-aa21-425d820da886)<br>
---

### Custom Domain Setup Using Cloudflare

1. Connect your domain & sudomain to the application using Cloudflare.<br> 
   e.g. - Domain: tanujbhatia.site<br>
   e.g. - Subdomain: api.tanujbhatia.site<br>
   
2. Create a CNAME record pointing to the load balancer endpoint.<br>
   <img width="446" alt="image" src="https://github.com/user-attachments/assets/36cab0cd-c2c6-4c7a-b60d-f65bb64bca30" />

3. Update .env & urls.js in frontend to use your new domain/subdomain.
   ```bash
   # Go to frontend directory
   cd ~/TravelMemory/frontend

   # Create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements): 
   REACT_APP_BACKEND_URL=http://backend_api_url #e.g. - http://api.tanujbhatia.site

   # Build the dependencies
   npm run build

   # Start the application
   sudo npm start
   ```
4. Now we can access the application using domain name because of DNS settings.<br>
   ![image](https://github.com/user-attachments/assets/f12ae51c-e6ed-4e85-bb9f-41fa3911672d)<br>

   Click on "Add Experience" and enter new data through UI.<br>
   <img width="956" alt="image" src="https://github.com/user-attachments/assets/4d37bc15-44a2-4c4a-af48-a2d4630c58f6" /><br>
   
   The data get stored in the MongoDB database.<br>
   ![image](https://github.com/user-attachments/assets/35a49355-3ec9-4dd5-8c9a-8f520830fc55)
---

### Deployment Status

1. Backend on EC2 with Nginx (port 80).
2. Frontend on EC2 with Nginx (port 80).
3. MongoDB connected via Atlas.
4. Load balancing enabled (multi-AZ support).
5. Domain/subdomain setup complete.
