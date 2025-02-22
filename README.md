# AWS Deployment of Multi-Tier Java Web Application

## Project Overview
This project sets up a multi-tier Java web application on AWS using the following components:
- **Database Layer**: MariaDB (MySQL EC2 instance)
- **Caching Layer**: Memcached EC2 instance
- **Message Broker**: RabbitMQ EC2 instance
- **Application Layer**: Tomcat EC2 instances in an Auto Scaling Group
- **Load Balancer**: AWS Application Load Balancer
- **Reverse Proxy**: NGINX
- **Artifact Storage**: S3 bucket
- **Domain Management**: Route 53 (public & private zones)
- **Security Groups**: Configured for each layer
- **Provisioning**: Vagrant for VM automation

## Flow of Execution
1. **Login to AWS Account**
2. **Create Key Pairs** for secure SSH access to instances
3. **Create Security Groups** to control access between components
4. **Launch EC2 Instances** with user data using BASH scripts:
   - `tomcat_ubuntu.sh`
   - `nginx.sh`
   - `mysql.sh`
   - `memcache.sh`
   - `rabbitmq.sh`
5. **Update IP to Name Mapping in Route 53** (Public and Private DNS zones)
6. **Build Application from Source Code** using Maven
7. **Upload Built Artifact to S3 Bucket** for deployment
8. **Download Artifact to Tomcat EC2 Instances** from S3
9. **Set up ELB with HTTPS** using a certificate from AWS Certificate Manager
10. **Map ELB Endpoint to Website Name in GoDaddy DNS**
11. **Verify Deployment** by testing the application
12. **Create an Auto Scaling Group** for Tomcat instances to ensure high availability

## File Structure
```
src/
├── main/
│   ├── java/com/visualpathit/account/
│   ├── resources/
│   ├── webapp/
│
├── test/java/com/visualpathit/account/
│
├── userdata/
│   ├── application.properties
│   ├── backend.sh
│   ├── memcache.sh
│   ├── mysql.sh
│   ├── nginx.sh
│   ├── rabbitmq.sh
│   ├── tomcat_ubuntu.sh
│
├── README.md
├── pom.xml
```

## Deployment Diagram
Refer to the provided architecture diagram for a visual representation of the AWS infrastructure setup.

## Key Configurations
- **Tomcat Instances** pull the application artifact from S3 at startup
- **Memcached** caches frequently accessed data for performance optimization
- **RabbitMQ** handles messaging between application components
- **MariaDB (MySQL)** stores application data
- **NGINX** serves as a reverse proxy for handling requests
- **Load Balancer** distributes traffic across Tomcat instances
- **Auto Scaling** ensures high availability of application instances

## Prerequisites
- AWS account with IAM permissions
- Vagrant installed for local provisioning
- AWS CLI configured for S3 and Route 53 operations
- Maven installed for building the Java application

## How to Deploy
1. Clone the repository:
   ```bash
   git clone <repo-url>
   cd <repo-directory>
   ```
2. Configure AWS credentials using AWS CLI:
   ```bash
   aws configure
   ```
3. Run Vagrant to provision local VMs (if applicable):
   ```bash
   vagrant up
   ```
4. Execute BASH scripts inside `userdata/` directory to set up instances.
5. Deploy the application using the steps outlined in the "Flow of Execution".

## Verification
After deployment, access the application using the ELB endpoint or domain name mapped via GoDaddy DNS.

## Conclusion
This setup ensures a scalable, highly available, and well-structured AWS deployment of a multi-tier Java web application.

