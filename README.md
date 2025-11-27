🚀 Java Spring Boot Project – Deployed Application (AWS Full Stack Deployment)
<img width="1000" height="750" alt="image" src="https://github.com/user-attachments/assets/cbad1be5-f0b0-4c7d-b2ab-b9db5f6be85b" />
##output###

<img width="1920" height="631" alt="11" src="https://github.com/user-attachments/assets/8cd8bdd8-8387-45ff-ad41-46a1f9685adb" />
<img width="1920" height="1080" alt="12" src="https://github.com/user-attachments/assets/28a133aa-c5e1-4dc3-919b-e68c875400c5" />
<img width="1920" height="1080" alt="13" src="https://github.com/user-attachments/assets/c8e63591-ca37-4b07-a9dc-ca87a0f5be33" />
<img width="1920" height="1080" alt="14" src="https://github.com/user-attachments/assets/2049258c-6c1b-495a-a5f1-d063f9251cb0" />

🟦 What Spring Boot Does

✔️ It simplifies Java development by:

Removing complex configuration

Starting applications with a single command (java -jar app.jar)

Embedding a server like Tomcat/Jetty, so no need to deploy WAR files

Providing built-in tools for security, databases, REST APIs, metrics, etc.



######java-spring-boot-projrct-deployed-application######

###👉  🟦 1️⃣ Create Amazon RDS (MySQL)  👉 Click “Create database”

                                       👉 Choose a database engine -MySQL , full configuration
                                            
                                       👉 Choose version - MySQL 8.0.x
                                       
                                       👉 Choose a Template- Free tier (if eligible) , Dev/Test(Production)
        
                                       👉 Choose a Master username - admin 
  
                                       👉 Choose a Master password - self managed - Set a strong password.Confirm the password

                                       👉 Configure Instance Size - db t3.micro

                                       👉 Connectivity - vpc(default) , subnet group , security group

                                       👉 Choose a accessible - public
                                      
                                       👉 Choose Create Database 


##### 🟧 2️⃣ Create Two EC2 Instances #####

 #👉 create ec2- frontend-t3.micro ,procced with keypair ,attach IAM role(ec2-fullaccess), security group 

 #👉 create ec2- backendend-t3.micro ,procced with keypair ,attach IAM role(ec2-fullaccess), security group



👉 ###🟪 3️⃣ Backend Deployment (Java + Spring Boot)
✔ Connect to Backend EC2###
 
      
                                👉 sudo su -
                   
                                👉 yum install git -y
             
                                👉 git clone https://github.com/CloudTechDevOps/Java-springboot-project.git

## we need install maven(root)  👉 yum install maven -y

###after complete this showing like :

: [INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
   
                                👉 ls like : Java-springboot-project
      
                                👉 cd Java-springboot-project 
        
 (inside)Java-springboot-project 👉 (install maven-package) 👉 mvn clean package -Dspring.profiles.active=build  # run this command jar.file are create go to (cd target)
 
 move jar.file into root [root@ip-172-31-79-9 ~]#      👉 mv datastore-0.0.7.jar /root/

then run this one in root[root@ip-172-31-79-9 ~]#      👉MYSQL_HOST="jdbc:mysql://database-1.cmryeuiue0wo.us-east-1.rds.amazonaws.com:3306/datastore?createDatabaseIfNotExist=true" \
                                                          MYSQL_USERNAME="admin" \
                                                          MYSQL_PASSWORD="samir123" \
                                                          nohup java -jar datastore-0.0.7.jar > /var/log/app/nohup.out 2>&1 &

                                      showing like this 👉 [1] 27021
 
              
           run this command root 👉  ps aux | grep jar
                                
                                 👉 cat /var/log/app/nohup.out

            ###showing like this 👉  nohup: ignoring input

 .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.6.4)

### backend part is done ###



👉 ###🟪 3️⃣ frontend Deployment (Python + Streamlit)
✔ Connect to frontend EC2###
 
      
                                👉 sudo su -
                   
                                👉 yum install git -y
             
                                👉 git clone https://github.com/CloudTechDevOps/Java-springboot-project.git

                                👉 ls like : Java-springboot-project

                                👉 cd Java-springboot-project -   (src,traget,dockerfile-1,fontend,process)
  
                                👉 cd frontend inside (app.py,process)
                                
  do with frontend inside       👉 yum install python3-pip  #install python

                     [frontend] 👉 python3 -m venv venv

              (venv) [frontend] 👉 source venv/bin/activate 

                     [frontend] 👉 pip install --upgrade pip

                     [frontend] 👉 pip install streamlit requests

                     [frontend] 👉 vi /etc/systemd/system/frontend.service

                     [frontend] 👉 [Unit]  # paste this code inside  vi /etc/systemd/system/frontend.service 

                                👉    Description=Streamlit Frontend App
                                       After=network.target
                        
                                       [Service]
                                       User=root
                                       WorkingDirectory=/root/Java-springboot-project/frontend
                 ExecStart=/root/Java-springboot-project/frontend/venv/bin/python -m streamlit run /root/Java-springboot-project/frontend/app.py --server.port=8501 --server.address=0.0.0.0
                                       Environment=API_URL=http://<backend pvt:ip>:8084
                                       Restart=always
                                       RestartSec=5

                                       [Install]
                                       WantedBy=multi-user.target  


run this command in  [frontend] 👉   systemctl daemon-reload
                    
                     [frontend] 👉   systemctl enable frontend

                     [frontend] 👉   systemctl start frontend

                     [frontend] 👉   systemctl status frontend 

# status showing active: frontend part is done ### copy frontend publicip hit brower :## publicip:8501 port


    ✅ ✅java_spring_boot_deployed_application successfully |||
