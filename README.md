🚀 Java Spring Boot Project – Deployed Application (AWS Full Stack Deployment)

A complete end-to-end Java Spring Boot backend, Streamlit frontend, and AWS cloud deployment project using RDS, EC2, IAM, Maven, and systemd.

📌 Project Overview

This project demonstrates:

Java Spring Boot REST API (Backend)

Streamlit Python Frontend

MySQL Amazon RDS Database

Deployment on AWS EC2 Linux Servers

Running Backend using java -jar

Running Frontend with systemd Service

Git, Maven, IAM Role, Security Groups, Virtual Environments

🟩 Technologies Used

Java 11 / 17

Spring Boot 2.6.x

MySQL 8.x (RDS)

Python3 + Streamlit

AWS EC2

AWS RDS (MySQL)

Amazon Linux / CentOS

Maven

Systemd

🟦 1️⃣ Create Amazon RDS (MySQL)
👉 Steps:

Go to AWS RDS → Create database

Engine: MySQL

Version: MySQL 8.0.x

Template: Free Tier / Dev Test

Master username: admin

Master password: (your password)

Instance class: db.t3.micro

Connectivity:

VPC: default

Subnet group: default

Public access: YES

Security group: allow 3306

Click Create Database

Copy the DB endpoint:

database-1.xxxxxx.us-east-1.rds.amazonaws.com

🟧 2️⃣ Create Two EC2 Instances
EC2 1 — Frontend Server

Instance: t3.micro

IAM Role: AmazonEC2FullAccess

Security Group: Allow port 8501

EC2 2 — Backend Server

Instance: t3.micro

IAM Role: AmazonEC2FullAccess

Security Group: Allow port 8084

🟪 3️⃣ Backend Deployment (Java + Spring Boot)
✔ Connect to Backend EC2
sudo su -
yum install git -y
git clone https://github.com/CloudTechDevOps/Java-springboot-project.git

✔ Install Maven
yum install maven -y

✔ Build application
cd Java-springboot-project
mvn clean package -Dspring.profiles.active=build


After success, JAR appears in:

/Java-springboot-project/target/datastore-0.0.7.jar


Move JAR to /root/:

mv target/datastore-0.0.7.jar /root/

✔ Run Spring Boot App
MYSQL_HOST="jdbc:mysql://database-1.xxxxx.us-east-1.rds.amazonaws.com:3306/datastore?createDatabaseIfNotExist=true" \
MYSQL_USERNAME="admin" \
MYSQL_PASSWORD="yourpassword" \
nohup java -jar datastore-0.0.7.jar > /var/log/app/nohup.out 2>&1 &


Check running process:

ps aux | grep jar


Check logs:

cat /var/log/app/nohup.out


You should see:

 .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.6.4)

Backend successfully deployed ✔

🟩 4️⃣ Frontend Deployment (Python + Streamlit)
✔ Connect to Frontend EC2
sudo su -
yum install git python3-pip -y
git clone https://github.com/CloudTechDevOps/Java-springboot-project.git

✔ Set up Python environment
cd Java-springboot-project/frontend
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install streamlit requests

🟦 5️⃣ Create systemd Service for Streamlit

Create service file:

vi /etc/systemd/system/frontend.service


Paste this:

[Unit]
Description=Streamlit Frontend App
After=network.target

[Service]
User=root
WorkingDirectory=/root/Java-springboot-project/frontend
ExecStart=/root/Java-springboot-project/frontend/venv/bin/python -m streamlit run app.py --server.port=8501 --server.address=0.0.0.0
Environment="API_URL=http://<BACKEND_PRIVATE_IP>:8084"
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target

🟧 6️⃣ Start Streamlit Frontend
systemctl daemon-reload
systemctl enable frontend
systemctl start frontend
systemctl status frontend


Status MUST show active (running) ✔

Access the frontend in a browser:
http://<FRONTEND_PUBLIC_IP>:8501

🎉 Deployment Completed Successfully

Both frontend and backend are now live.

✔ Java Spring Boot API running on backend EC2
✔ Streamlit UI running on frontend EC2
✔ MySQL running on AWS RDS
✔ Fully deployed cloud application

⭐ Project Completed Successfully
java_spring_boot_deployed_application successfully deployed ✔✔✔
