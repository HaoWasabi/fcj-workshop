---
title : "Create ECS Cluster and Service"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.4.8. </b> "
---

After completing this section, the system will meet the following requirements:

- Successfully create an ECS Cluster using **AWS Fargate**.
- Configure the ECS Service to use Subnets across **2 Availability Zones (AZ1, AZ2)** to improve availability.
- Create Task Definitions for Backend and Frontend with **256** CPU and **512 MB** RAM.
- Configure CloudWatch Logs for containers.
- Set up environment variables and Secrets for the application.
- Configure an IAM Task Execution Role that allows ECS to pull Docker Images from Amazon ECR and write logs to CloudWatch.
- Verify that tasks start and run correctly.

---

### Steps

#### Step 1. Create an ECS Cluster using AWS Fargate

1. Go to the **AWS Management Console**.

2. Search for and select **Amazon ECS**.

3. In the left menu, select **Clusters**.

4. Click **Create cluster**.

5. In the **Cluster name** field, enter:

```text
neonfoodmap-cluster
```

6. Under **Infrastructure**, select:

```text
AWS Fargate (serverless)
```

7. Review the cluster configuration.

8. Click **Create** to create the cluster.

9. Once created successfully, the cluster will appear in the cluster list.

> **Note:** An ECS Cluster is a logical resource and is not directly assigned to AZ1 or AZ2. Multi-AZ deployment is configured at the ECS Service level by selecting Subnets that belong to both AZ1 and AZ2.

---

![](/images/5-Workshop/5.4-Neon-Deployment/image010.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image011.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image012.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image013.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image014.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image015.png)

#### Step 2. Create a Task Definition for Backend with CPU 256 and RAM 512 MB

A Task Definition defines how ECS launches the Backend Container, including the Docker Image, CPU, RAM, Port, Environment Variables, and Log Configuration.

1. In Amazon ECS, select **Task definitions**.

2. Click **Create new task definition**.

3. Enter the Task Definition name:

```text
neonfoodmap-backend
```

4. Under **Infrastructure requirements**, select:

```text
AWS Fargate
```

5. Configure the resource allocation:

```text
CPU: 0.25 vCPU
Memory: 0.5 GB
```

Which corresponds to:

```text
CPU: 256
RAM: 512 MiB
```

6. Under **Task execution role**, select the IAM Role for ECS Task Execution.

Example:

```text
NeonFoodmap-ECS-TaskExecution-Role
```

7. Under the **Container** section, click **Add container**.

8. Enter the container name:

```text
backend
```

9. In the **Image URI** field, enter the Backend Docker Image URI stored on Amazon ECR.

Example:

```text
<AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/<BACKEND_REPOSITORY>:latest
```

10. Configure **Port mapping** based on the port used by the Backend Container.

Example:

```text
Container port: 8000
Protocol: TCP
```

11. Configure the **Environment variables** required for the application.

Example:

```text
DJANGO_SETTINGS_MODULE
DEBUG
ALLOWED_HOSTS
AWS_REGION
```

12. For sensitive information such as RDS connection details or API Keys, configure them via **Secrets** rather than hardcoding values directly into the Task Definition.

13. Configure container logging using **Amazon CloudWatch Logs**.

Log Group:

```text
/ecs/neonfoodmap-backend
```

14. Review the following before creating:

- Docker Image.
- CPU and RAM.
- Container Port.
- Environment Variables.
- Secrets.
- Task Execution Role.
- CloudWatch Log Group.

15. Click **Create** to create the Task Definition.

---

![](/images/5-Workshop/5.4-Neon-Deployment/image016.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image017.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image018.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image019.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image020.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image021.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image022.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image023.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image024.png)

#### Step 3. Create a Task Definition for Frontend with CPU 256 and RAM 512 MB

Follow the same steps as the Backend, but use the Frontend Docker Image instead.

1. Navigate to:

**Amazon ECS → Task definitions → Create new task definition**.

2. Enter the Task Definition name:

```text
neonfoodmap-frontend
```

3. Select:

```text
AWS Fargate
```

4. Configure the resource allocation:

```text
CPU: 0.25 vCPU
Memory: 0.5 GB
```

5. Select the **Task execution role**:

```text
NeonFoodmap-ECS-TaskExecution-Role
```

6. Click **Add container**.

7. Enter the container name:

```text
frontend
```

8. Enter the Frontend Docker Image URI from Amazon ECR.

Example:

```text
<AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/<FRONTEND_REPOSITORY>:latest
```

9. Configure Port Mapping based on the port used by the Frontend Container.

Example:

```text
Container port: 80
Protocol: TCP
```

10. Configure the required Environment Variables for the Frontend.

11. Configure CloudWatch Logs with the Log Group:

```text
/ecs/neonfoodmap-frontend
```

12. Review the configuration and click **Create**.

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image054.png)

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image055.png)

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image056.png)

---

#### Step 4. Configure CloudWatch Log Groups for Backend and Frontend

CloudWatch Logs centralizes logs from ECS Containers, supporting activity monitoring and troubleshooting.

1. Go to the **Amazon CloudWatch** service.

2. Select **Logs → Log groups**.

3. Click **Create log group**.

4. Create the Log Group for Backend:

```text
/ecs/neonfoodmap-backend
```

5. Click **Create**.

6. Create the Log Group for Frontend:

```text
/ecs/neonfoodmap-frontend
```

7. Verify that both Log Groups appear in the list.

8. Confirm the Log Group names in the Task Definition match the ones created above.

| Container | CloudWatch Log Group |
|-----------|----------------------|
| Backend | `/ecs/neonfoodmap-backend` |
| Frontend | `/ecs/neonfoodmap-frontend` |

> When an ECS Task starts, container logs will be forwarded to the corresponding Log Group via the `awslogs` configuration.

---

#### Step 5. Set up Environment Variables and Secrets for RDS and API Keys

Application configuration is divided into two groups: **Environment Variables** and **Secrets**.

##### 5.1. Configure Environment Variables

In the Backend Task Definition, under the **Environment variables** section, add configuration variables that do not contain sensitive information.

Example:

```text
DJANGO_SETTINGS_MODULE=config.settings
DEBUG=False
ALLOWED_HOSTS=<DOMAIN>
AWS_REGION=<AWS_REGION>
```

Replace these values with the actual values for your deployment environment.

##### 5.2. Configure RDS Connection Details

Database connection details may include:

```text
DB_HOST
DB_NAME
DB_USER
DB_PASSWORD
DB_PORT
```

Sensitive values such as `DB_PASSWORD` should be stored in **AWS Secrets Manager**.

In the Task Definition:

1. Open the **Secrets** section of the Backend Container.

2. Click **Add secret**.

3. Enter the name of the Environment Variable used by the application.

4. Select the corresponding Secret from **AWS Secrets Manager**.

Example:

```text
Name: DB_PASSWORD
ValueFrom: <RDS Database Secret>
```

##### 5.3. Configure API Keys

For API Keys or credentials for external services, follow the same approach:

```text
Name: API_KEY
ValueFrom: <API Key Secret>
```

5. Review all declared environment variables and Secrets.

6. Do not hardcode RDS passwords or API Keys in source code, Dockerfiles, or any configuration files committed to Git.

> The task must be granted appropriate permissions to access Secrets from AWS Secrets Manager.

---

#### Step 6. Create a Task Execution Role with ECR Access

The Task Execution Role allows ECS to perform the necessary operations when launching a Task.

1. Go to **AWS Console → IAM**.

2. Select **Roles**.

3. Find the Role:

```text
NeonFoodmap-ECS-TaskExecution-Role
```

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image057.png)

4. Open the Role and select the **Permissions** tab.

5. Verify that the Role has the necessary permissions to:

- Pull Docker Images from Amazon ECR.
- Write Container Logs to Amazon CloudWatch Logs.
- Access Secrets from AWS Secrets Manager if the Task Definition uses Secrets.

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image058.png)

6. For ECR access, the Role must have the corresponding permissions so that ECS/Fargate can authenticate and pull Images from the Repository.

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image060.png)

7. Go back to **Amazon ECS → Task definitions**.

8. Open the Backend Task Definition.

9. Verify that the **Task execution role** is set to:

```text
NeonFoodmap-ECS-TaskExecution-Role
```

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image061.png)

10. Repeat the same verification for the Frontend Task Definition.

---

### Verification

After completing the 6 steps above, review the configuration before creating the ECS Service:

| Component | Expected Result |
|-----------|-----------------|
| ECS Cluster | Created and in `ACTIVE` state |
| Backend Task Definition | Created with CPU 256 and RAM 512 MiB |
| Frontend Task Definition | Created with CPU 256 and RAM 512 MiB |
| Backend Image | Points to the Backend Repository on ECR |
| Frontend Image | Points to the Frontend Repository on ECR |
| CloudWatch Logs | Has `/ecs/neonfoodmap-backend` and `/ecs/neonfoodmap-frontend` |
| Environment Variables | Configured |
| Secrets | Configured via Secrets Manager |
| Task Execution Role | Assigned to both Backend and Frontend |
| ECR Permission | Allows ECS to pull Images |

Once all configurations above are complete, the system is ready to create the **ECS Service** and deploy Tasks across Subnets in **AZ1 and AZ2**.

![](/images/5-Workshop/5.4-Neon-Deployment/image039.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image040.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image041.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image042.png)
![alt text](/images/5-Workshop/5.4-Neon-Deployment/image062.png)
