---
layout: default
title: aws-elb
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-elb
nav_order: 8
---
# AWS Elastic Load Balancer (ELB) Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## AWS Elastic Load Balancer (ELB):

A Load Balancer is a networking device or service that distributes incoming network traffic (such as web requests or application data) 
across multiple servers or instances to ensure high availability, reliability, and optimal performance. The primary purpose of a load balancer 
is to evenly distribute the incoming requests and traffic to the backend servers, preventing any single server from being overwhelmed and improving 
the overall efficiency of the system. Load balancers are commonly used in web applications, websites, and other distributed systems to achieve fault
tolerance, scalability, and redundancy.

**AWS Elastic Load Balancer (ELB)**, often referred to simply as AWS ELB, is Amazon Web Services' fully managed load balancing service. ELB is 
designed to automatically distribute incoming application traffic across multiple Amazon Elastic Compute Cloud (EC2) instances, containers, and 
other resources within an AWS Virtual Private Cloud (VPC). AWS ELB is highly available, scalable, and integrates seamlessly with other AWS services, 
making it a popular choice for load balancing in cloud-based applications.

There are four classes of AWS ELB:

1. **Application Load Balancer (ALB)**: Designed for distributing HTTP and HTTPS traffic, ALB offers advanced routing capabilities and content-based routing, making it suitable for web applications and microservices.

2. **Network Load Balancer (NLB)**: Optimized for handling TCP and UDP traffic, NLB provides high-performance, low-latency load balancing with support for static IP addresses, and it preserves the source IP address of the client. It is an excellent choice for applications that require high performance and low latency.

3. **Classic Load Balancer (deprecated)**: The Classic Load Balancer is a legacy option supporting both HTTP/HTTPS and TCP/UDP traffic. It provides basic load balancing capabilities and is recommended for existing users to migrate to ALB or NLB for enhanced features and performance.

4. **Gateway Load Balancer**: Gateway Load Balancer helps you easily deploy, scale, and manage your third-party virtual appliances. It gives you one gateway for distributing traffic across multiple virtual appliances while scaling them up or down, based on demand. This decreases potential points of failure in your network and increases availability.

AWS ELB automatically routes traffic to healthy instances, monitors the health of the backend resources, and scales based on demand. It also offers 
features like SSL termination, sticky sessions, security group integration, and access logs. ELB plays a crucial role in ensuring the availability 
and reliability of applications hosted on AWS by distributing traffic across multiple instances or resources.

## ELB Classes:

1. **Application Load Balancer (ALB)**:
   - Designed for HTTP/HTTPS traffic and provides advanced routing, content-based routing, and support for containers.
   - Supports routing based on host, path, query parameters, and more.
   - Best suited for web applications and microservices.

2. **Network Load Balancer (NLB)**:
   - Ideal for TCP/UDP traffic and provides high-performance, low-latency load balancing.
   - Supports static IP addresses and preserves the source IP of the client.
   - Best for applications requiring extreme performance.

3. **Classic Load Balancer** (Deprecated - Consider migrating to ALB or NLB):
   - Supports both HTTP and TCP/UDP traffic.
   - Provides basic load balancing capabilities.
   - Legacy load balancer class, consider migrating to ALB or NLB for enhanced features.
   
4. **Gateway Load Balancer**:
   - xxx
   - xxx
   - xxx

## Quotas and Constraints:

- **Application Load Balancer (ALB)**:
  - Max Listeners per Load Balancer: 75
  - Max Target Groups per Load Balancer: 75
  - Max Rules per Listener: 50
  - Max URLs per Rule: 100
  - Idle Connection Timeout: 60 seconds

- **Network Load Balancer (NLB)**:
  - Max Listeners per Load Balancer: 25
  - Max Target Groups per Load Balancer: 25
  - Idle Connection Timeout: Configurable (Default: 350 seconds)
  - Max Elastic IP addresses: Per your AWS account limits

- **Classic Load Balancer** (Deprecated):
  - Max Listeners per Load Balancer: 1
  - Max Instances per Load Balancer: 100
  - Idle Connection Timeout: 60 seconds

- **Gateway Load Balancer**
  - xxx
  - xxx
  - xxx


## AWS ELB Rules:

### 1. Listener Rules:

- **Description**: Listener rules define how incoming traffic is routed to target groups. They are specific to Application Load Balancers (ALB).

- **Key Elements**:
  - **Priority**: Numeric order of rule evaluation (lower numbers are evaluated first).
  - **Conditions**: Define conditions for the rule to match (e.g., path, host, query string).
  - **Actions**: Specify the target group to route matched traffic to.

- **Example**:
  - Priority: 1
  - Condition: If the path is '/api', route to Target Group A.
  - Priority: 2
  - Condition: If the host is 'api.example.com', route to Target Group B.

### 2. Target Group Rules:

- **Description**: Target group rules determine how traffic within a target group is routed to registered instances. Specific to ALB.

- **Key Elements**:
  - **Path Patterns**: Define URL path patterns to route to different instances.
  - **Path Patterns Conditions**: Match specific paths and route accordingly.
  - **Weighted Routing**: Assign weights to instances for proportional traffic distribution.

- **Example**:
    - Path Pattern: /images/* -> Route to instances in Target Group A.
    - Path Pattern: /videos/* -> Route to instances in Target Group B.
    - Weighted Routing: 75% to Target Group C, 25% to Target Group D.

### 3. Host-Based Routing:

- **Description**: Specific to ALB, allows routing based on the host header in HTTP requests.

- **Use Cases**: Route traffic to different applications or microservices based on the domain name in the URL.

- **Example**:
  - Route traffic for example.com to Target Group A.
  - Route traffic for api.example.com to Target Group B.

### 4. Path-Based Routing:

- **Description**: Specific to ALB, allows routing based on the URL path.

- **Use Cases**: Route traffic to different paths or endpoints within your application.

- **Example**:
  - Route traffic for /app1/* to Target Group A.
  - Route traffic for /app2/* to Target Group B.

### 5. Query String Rules:

- **Description**: Specific to ALB, allows routing based on query string parameters.

- **Use Cases**: Route traffic based on specific query parameters or values.

- **Example**:
  - Route traffic with ?product=123 to Target Group A.
  - Route traffic with ?category=books to Target Group B.

### 6. Redirect Rules:

- **Description**: Specific to ALB, allows creating URL redirects.

- **Use Cases**: Implement URL redirections for specific paths or URLs.

- **Example**:
  - Redirect /old-path to /new-path with a 301 status code.

### 7. Fixed Response Rules:

- **Description**: Specific to ALB, allows generating fixed responses for specific requests.

- **Use Cases**: Send predefined responses for certain requests, like custom error pages.

- **Example**:
  - Return a custom "Not Found" HTML page with a 404 status code.

These are some of the key rule types and elements you can configure in an AWS Application Load Balancer (ALB) to control how traffic is routed to your target groups and instances. Rules 
play a critical role in enabling advanced routing, load balancing, and customization for your applications hosted on AWS ELB.

## Cross-Zone Load Balancing:

### 1. Cross-Zone Load Balancing:

- **Description**: Cross-Zone Load Balancing is a feature available for both Application Load Balancers (ALB) and Network Load Balancers (NLB).

- **Purpose**: Distributes traffic evenly across all instances in all availability zones rather than segregating traffic to specific zones.

- **Benefits**:
  - Improves traffic distribution and fault tolerance.
  - Helps in distributing traffic to instances across availability zones for better utilization.

### 2. Enabling Cross-Zone Load Balancing:

- **ALB**:
  - Enabled by default; no action needed.

- **NLB**:
  - Enabled when creating or modifying an NLB.

## Stickiness:

### 1. Stickiness:

- **Description**: Stickiness, also known as session affinity, ensures that a client's requests are directed to the same target instance during the session.

- **Purpose**: Useful for applications that require session persistence, such as maintaining state for shopping carts or user sessions.

- **Types**:
  - **Application Load Balancer (ALB)**: Supports sticky sessions using cookies.
  - **Classic Load Balancer (CLB)**: Supports cookie-based session stickiness.

### 2. Cookie-Based Stickiness (ALB):

- **Key Components**:
  - **Duration**: Set the time (in seconds) for the stickiness. The session remains sticky during this period.
  - **Stickiness Policy**: ALB generates a session cookie for clients and associates requests with the same cookie value to the same target instance.

- **Use Cases**:
  - E-commerce websites to maintain a consistent shopping cart experience.
  - Applications requiring user-specific sessions.

### 3. Application Load Balancer (ALB) Stickiness:

- **Enabling Stickiness**:
  - When configuring a target group, set the **stickiness policy**.

### 4. Classic Load Balancer (CLB) Stickiness:

- **Description**: Classic Load Balancers support stickiness using a custom-generated cookie.

- **Enabling Stickiness**:
  - When configuring the CLB, set the **Enable** option for session stickiness.
  - Specify the **stickiness policy name**, **cookie name**, and the **cookie expiration period**.

- **Use Cases**:
  - Applications that need custom stickiness policies and session management.

### 5. NLB Stickiness:

- **Description**: NLBs do not support built-in session stickiness. You must implement stickiness at the application level for NLBs.

- **Use Cases**:
  - Implement custom session management at the application layer.

Stickiness and Cross-Zone Load Balancing are essential features when configuring AWS Elastic Load Balancers to ensure that traffic is distributed effectively and that user sessions are maintained 
consistently. Use these features based on your application's requirements for session persistence and load balancing.

## Connection draining 

Connection draining is a feature provided by Amazon Web Services (AWS) Elastic Load Balancers (ELB) that allows the load balancer to gracefully complete in-flight requests made to instances that are 
being taken out of service due to scaling activities, instance health issues, or maintenance. It ensures that no new connections are sent to the instances being deregistered, allowing them to complete 
their existing requests, and it's particularly useful for maintaining application availability during scaling or maintenance activities.

Here's how connection draining works:

1. **Detecting the Need for Draining**:
    - The need for connection draining arises when an instance is marked for removal from the load balancer. This can happen during Auto Scaling events, instance health checks, or manual interventions.

2. **Enabling Connection Draining**:
    - The load balancer is configured with a "connection draining" setting, which specifies the maximum time allowed for connection draining. When enabled, the load balancer enters "draining" mode for instances marked for removal.

3. **Draining Mode**:
    - When connection draining is enabled, the load balancer continues to serve requests to instances marked for removal.
    - The load balancer stops sending new connections to these instances while allowing existing connections to complete.

4. **Monitoring and Completing Requests**:
    - During the connection draining period, the load balancer closely monitors the instances' status.
    - As the number of in-flight requests on these instances decreases, the instances are considered healthy once the requests are completed.

5. **Instance Deregistration**:
    - Once the load balancer determines that the instances have completed draining (i.e., no more in-flight requests), it can safely deregister them.

6. **Traffic Redistribution**:
    - After an instance has been deregistered and drained, the load balancer resumes sending new connections to the remaining healthy instances.
    - The load balancer's traffic distribution among the healthy instances is based on its routing algorithm, such as round-robin or least connections.

Key Benefits of Connection Draining:

- **Improved User Experience**: Connection draining helps ensure that users' requests are not abruptly terminated when instances are taken out of service. This leads to a more seamless user experience.

- **Graceful Scaling**: During Auto Scaling events, connection draining allows the removal of instances without impacting the ongoing user interactions. This enables you to efficiently manage your application's scaling requirements.

- **Effective Maintenance**: When performing maintenance on instances, connection draining ensures that you can complete in-flight requests before shutting down or updating instances.

It's important to configure connection draining settings based on your specific application's requirements, taking into account the expected duration of in-flight requests and the capacity of the 
instances being drained. Connection draining is particularly useful for maintaining application availability and ensuring a smooth transition during scaling and maintenance activities.

## SSL/TLS on AWS Elastic Load Balancer (ELB):

SSL/TLS (Secure Sockets Layer/Transport Layer Security) on AWS Elastic Load Balancer (ELB) is used to secure the communication between clients and the load balancer and between the load balancer 
and the backend instances. ELB supports SSL/TLS termination, allowing it to handle the encryption and decryption of traffic on behalf of the backend instances. Here's how SSL/TLS works on AWS ELB:

1. **SSL/TLS Termination**:
   - ELB acts as an SSL/TLS endpoint for incoming client requests.
   - Clients establish an encrypted connection with the ELB using SSL/TLS.

2. **Load Balancer to Instance Communication**:
   - ELB decrypts incoming traffic and forwards unencrypted requests to the backend instances.
   - Backend instances handle the unencrypted traffic, which can be more efficient.

3. **Encryption Offload**:
   - ELB offloads the resource-intensive encryption and decryption tasks from backend instances.
   - This allows instances to focus on processing application logic.

4. **Security and Configuration**:
   - ELB supports SSL/TLS protocols and ciphers for secure connections.
   - SSL certificates and private keys are managed and stored on ELB for secure communication.

5. **Security Policies**:
   - ELB provides predefined SSL policies with recommended security settings.
   - You can select an appropriate security policy based on your security and compliance requirements.

6. **Optional Client Authentication**:
   - ELB can be configured to require client authentication using client certificates.
   - This adds an extra layer of security but is optional and depends on your use case.

## AWS ELB SSL/TLS:

### 1. SSL/TLS Termination:

- **Description**: ELB terminates SSL/TLS connections, offloading encryption/decryption from backend instances.

- **Benefits**:
    - Reduces the CPU load on backend instances.
    - Simplifies SSL certificate management.
    - Supports SSL/TLS for secure communication.

### 2. Supported SSL/TLS Protocols:

- **Protocols**: ELB supports various SSL/TLS protocols, including TLS 1.2 and 1.3.
- **Ciphers**: ELB provides predefined security policies with recommended ciphers.

### 3. SSL/TLS Certificates:

- **Upload Certificates**: You can upload SSL/TLS certificates to ELB for secure communication.
- **Certificate Management**: AWS Certificate Manager (ACM) can be used to manage SSL/TLS certificates.

### 4. Security Policies:

- **Predefined Policies**: ELB provides predefined security policies for SSL/TLS with recommended configurations.
- **Custom Policies**: You can create custom security policies to configure SSL/TLS settings based on your specific needs.

### 5. Listener Configuration:

- **Listeners**: Define SSL/TLS listeners on ELB.
- **Port Mapping**: Map SSL/TLS listeners to target groups with instances.
- **Backend Protocol**: Define the protocol used between the load balancer and backend instances.

### 6. Client Authentication (Optional):

- **Client Certificates**: ELB can be configured to require client authentication using client certificates.
- **Additional Security**: Provides an extra layer of security for client-to-load balancer communication.

Securely configuring SSL/TLS on AWS ELB is essential for protecting sensitive data in transit. Be sure to select appropriate SSL policies, 
manage certificates, and configure listeners to meet your security and compliance requirements.

## Step-by-step guide to create an SSL/TLS certificate using AWS Certificate Manager (ACM)

**Step 1: Log in to the AWS Management Console**

- Go to the AWS Management Console (https://aws.amazon.com/).
- Sign in to your AWS account.

**Step 2: Open the ACM Console**

- In the AWS Management Console, search for "ACM" in the services search bar, and click on "AWS Certificate Manager."

**Step 3: Request a Certificate**

- In the ACM dashboard, click the "Request a certificate" button.

**Step 4: Choose the Certificate Type**

- In the "Request a certificate" wizard, select the type of certificate you want. You can choose either "Public certificate" or "Private certificate."

**Step 5: Add Domain Names**

- On the "Add domain names" page, enter the domain names for which you want to obtain the certificate (e.g., www.example.com, example.com). You can add multiple domain names.

**Step 6: Configure Domain Validation**

- Choose the method for domain validation. ACM provides three methods:
    - Email validation (requires receiving emails at specified email addresses associated with the domain).
    - DNS validation (involves creating DNS records).
    - AWS Identity and Access Management (IAM) role (useful for wildcard certificates).

**Step 7: Review and Confirm**

- Review the domain names and validation methods.
- Click the "Next" button to proceed.

**Step 8: Review and Confirm the Certificate Request**

- Review the details of the certificate request.
- Click the "Confirm and request" button to submit the certificate request.

**Step 9: Validation**

- ACM will initiate the validation process based on the method you chose in step 6. Follow the instructions to validate your domain ownership.
- Once the validation is complete, the status of your certificate will change to "Issued."

**Step 10: Use the Certificate**

- After the certificate is issued and validated, you can use it with AWS services like Elastic Load Balancers (ELB), CloudFront, and more.
- You can also view and manage your certificates in the ACM dashboard.

Your SSL/TLS certificate is now issued and ready to use with your AWS resources. Remember to renew the certificate before it expires to ensure uninterrupted secure communication.


## Step-by-step guide to install and configure SSL/TLS on an ALB:

**Note**: Before you begin, make sure you have your SSL/TLS certificate and private key ready for upload. You can use a certificate issued by AWS Certificate Manager (ACM) or a certificate from an external Certificate Authority (CA).

1. **Log in to the AWS Management Console**:
    - Go to the AWS Management Console (https://aws.amazon.com/).
    - Sign in to your AWS account.

2. **Navigate to the EC2 Dashboard**:
    - From the AWS Management Console, go to the EC2 dashboard.

3. **Create or Select Your Load Balancer**:
    - In the EC2 dashboard, select "Load Balancers" from the navigation pane.
    - Create a new Application Load Balancer or select an existing one to which you want to add SSL/TLS.

4. **Add a Listener**:
    - In the ALB configuration, select the "Listeners" tab.

5. **Add a New Listener**:
    - Click the "View/edit rules" button for the listener to which you want to add SSL/TLS.

6. **Add a New Rule**:
    - Click the "+" icon on the listener to add a new rule.

7. **Configure the Rule**:
    - Give the rule a name and set the conditions based on your SSL certificate's requirements.
    - For HTTPS traffic, you can specify conditions such as the host or path.

8. **Add an Action**:
    - Under the "Actions" section of the rule, click the "Insert Rule" button.
    - Select "Fixed Response" as the action type.

9. **Configure the Fixed Response Action**:
    - For "Fixed Response," you can configure an HTTP response code and response body.
    - Use this to create a fixed response for HTTP to HTTPS redirection, for example.

10. **Save the Rule**:
    - Click the "Save" button to save the rule.

11. **Add an SSL/TLS Certificate**:
    - In the ALB configuration, select the "Listeners" tab again.
    - Click the "View/edit certificates" button for the listener you want to configure.

12. **Add a Certificate**:
    - Click the "Add Listener" button to add a certificate.

13. **Configure the Certificate**:
    - Choose the SSL certificate you want to use. You can select an ACM certificate or upload an external certificate.

14. **Configure the Security Policy**:
    - Select the SSL/TLS security policy that matches your security requirements.

15. **Edit the Listener**:
    - Go back to the "Listeners" tab and click the "View/edit rules" button for the listener.

16. **Add a New Rule for HTTPS Traffic**:
    - Create a new rule and set the conditions for HTTPS traffic, typically for port 443.
    - Specify the conditions, such as the host, path, or other criteria.

17. **Add an Action for Forwarding**:
    - Under the "Actions" section of the rule, add an action to forward the request to a target group.

18. **Choose the Target Group**:
    - Select the target group that contains the instances or resources you want to route the traffic to.

19. **Save the Rule**:
    - Click the "Save" button to save the rule.

20. **Review and Save the Configuration**:
    - Review your configuration, including the SSL certificate and listener rules.
    - Click the "Save" or "Update" button to save your ALB configuration.

Your ALB is now configured to use SSL/TLS for secure communication. It will terminate SSL/TLS connections and forward traffic to your backend instances over an encrypted channel. 
Make sure to test your configuration thoroughly to ensure that it works as expected.


## Step-by-Step Guide for Configuring ELB:

### Application Load Balancer (ALB):

1. **Create an Application Load Balancer**:
    - Choose the ALB class.
    - Define the listener configuration (HTTP/HTTPS).
    - Specify security groups and subnets.
    - Configure routing rules.

2. **Create Target Groups**:
    - Define target group settings (e.g., protocol and port).
    - Register instances or IP addresses to the target group.

3. **Set Up Listener Rules**:
    - Define routing rules based on path, host, or query parameters.
    - Map rules to target groups.

4. **Configure Health Checks**:
    - Define health check settings for target group.

5. **Access Logs and Monitoring**:
    - Enable access logs and CloudWatch monitoring for the ALB.

### Network Load Balancer (NLB):

1. **Create a Network Load Balancer**:
    - Choose the NLB class.
    - Define listener configuration (TCP/UDP).
    - Specify security groups and subnets.

2. **Create Target Groups**:
    - Define target group settings (e.g., protocol and port).
    - Register instances or IP addresses to the target group.

3. **Configure Load Balancer Settings**:
    - Set cross-zone load balancing, idle timeout, and client IP preservation.

4. **Access Logs and Monitoring**:
    - Enable access logs and CloudWatch monitoring for the NLB.

### Classic Load Balancer (Deprecated - Consider migrating):

1. **Create a Classic Load Balancer**:
    - Choose the Classic Load Balancer class.
    - Define listener configuration (HTTP/HTTPS or TCP/UDP).
    - Specify security groups and subnets.

2. **Register Instances**:
    - Register EC2 instances with the load balancer.

3. **Configure Health Checks**:
    - Define health check settings for instances.

4. **Set Up Listener Policies**:
    - Define stickiness policies, if required.

5. **Access Logs and Monitoring**:
    - Enable access logs and CloudWatch monitoring for the Classic Load Balancer.

### Configuring an Application Load Balancer (ALB) with an Auto Scaling group:

**Prerequisites**:
- You should have your Amazon Machine Images (AMIs) prepared for your Auto Scaling instances.
- You should have created the necessary security groups, VPC, subnets, and IAM roles.
- Make sure you have an existing application or web server configuration that you want to deploy.

**Step 1: Create an Application Load Balancer (ALB)**

1. **Sign in to AWS Console**:
   - Go to the AWS Management Console.

2. **Navigate to EC2 Dashboard**:
   - From the AWS Management Console, go to the EC2 dashboard.

3. **Create an Application Load Balancer**:
   - In the EC2 dashboard, select "Load Balancers" from the navigation pane and click "Create Load Balancer."

4. **Configure Load Balancer Settings**:
   - Choose "Application Load Balancer."
   - Configure the listener settings, subnets, and security groups.
   - Configure routing rules to direct traffic to target groups.

5. **Create a New Target Group**:
   - Set up a target group for the ALB. This target group will be used to route traffic to your Auto Scaling group instances.

6. **Configure Health Checks**:
   - Set up health checks to monitor the status of your target instances.

7. **Review and Create the ALB**:
   - Review your configuration, and then create the ALB.

**Step 2: Create an Auto Scaling Group**

1. **Navigate to Auto Scaling**:
   - In the AWS Management Console, navigate to the Auto Scaling dashboard.

2. **Create an Auto Scaling Group**:
   - Click "Create Auto Scaling group."
   - Select your desired AMI and instance type.
   - Configure the Auto Scaling group settings, including the initial capacity and scaling policies.

3. **Configure Load Balancer**:
   - In the "Load balancing" section, select the ALB you created earlier.
   - Define the target group to route traffic to.

4. **Configure Scaling Policies**:
   - Create scaling policies based on triggers such as CPU utilization or network traffic.

5. **Set Up Notifications**:
   - Configure CloudWatch Alarms and SNS notifications for scaling events if desired.

6. **Configure Advanced Settings**:
   - Define advanced settings like instance protection, termination policies, and scaling cooldown periods.

7. **Review and Create the Auto Scaling Group**:
   - Review your configuration, and then create the Auto Scaling group.

**Step 3: Testing and Monitoring**

1. **Monitoring**:
   - Monitor the performance and health of your Auto Scaling group instances and your ALB using CloudWatch.

2. **Testing**:
   - Test your ALB by accessing your application using the ALB's DNS name.

**Step 4: Adjusting Scaling Policies**

1. **Fine-Tune Scaling Policies**:
   - Adjust your Auto Scaling group's scaling policies based on actual performance and traffic patterns.

2. **Scaling Out or In**:
   - You can manually adjust the desired capacity of your Auto Scaling group if necessary.
