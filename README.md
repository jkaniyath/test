# Spark ETL

## Integrate Azure Storage Container Gen2 with Azure Kubernetes Services.

    - Load the hotels and weather data frames from the Azure storage container.
    - Check the hotels data for incorrect (null) values in the Latitude and Longitude columns. For incorrect values, map the Latitude and Longitude from the OpenCage Geocoding API on the fly (via REST API).
    - Generate a geohash using Latitude and Longitude with one of the geohash libraries (like geohash-java) with a length of 4 characters in an extra column.
    - Perform a left join of the weather and hotels data using the generated 4-character geohash (avoid data multiplication and make your job idempotent).
    - Deploy the Spark job on the Kubernetes Service.

### Pre-requisites

    - Azure Subscription
    - Azure CLI
    - Install Docker
    - Install kubectl
    - Install Terraform
    - Install Hadoop and Spark
    - Install Python3
    - Install Java (preferably version 8 or 11).

### Deploy infrastructure with terraform

    Go to the folder where the Terraform file is located and type the following commands. All commands and their results are shown in the following screenshots.

    1. To initializes a Terraform working directory.
    ![init command](https://private-user-images.githubusercontent.com/165160551/344591540-3a5f7d63-c461-4053-ba5a-f09d1061636a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTk4MjEwOTcsIm5iZiI6MTcxOTgyMDc5NywicGF0aCI6Ii8xNjUxNjA1NTEvMzQ0NTkxNTQwLTNhNWY3ZDYzLWM0NjEtNDA1My1iYTVhLWYwOWQxMDYxNjM2YS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzAxJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcwMVQwNzU5NTdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02MzA3OWQzNDE2ZDlhNTcwZDMzNGRiYWVhOTVlZDAzN2JjODg1MmUwOWM1YjMwZGZlZTIxNDFkYTUyNTBmNTM1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.M8bkJR5mT3DTRGqLHlJF_DM39xk5leXclC19jXu94MQ)
    2. To create an execution plan, showing what actions Terraform will take to achieve the desired state described in the configuration files.
    ![plan command](plan.png)
    ![plan command result](plan-result.png)
    3. To apply the changes required to reach the desired state of the configuration.
    ![apply command](apply.png)
    ![apply command result](apply-result.png)

### Development and Testing

    You can perform development and testing either in a local environment or within a Docker container. In my case, I use a Docker container for these tasks. To set this up, you need to copy both your main Python files and your test files into the Docker container by specifying them in the Dockerfile. The code is organized in the src/main/ and src/tests/ directories in this repository.Below, I will show the Dockerfile creation and test results as screenshots.

    1. To create Docker image from a Dockerfile and a specified context.
    ![Docker build command](docker-build.png)
    2. To list all the Docker images.
    ![Docker list command](docker-images.png)
    3. To run a container from a specified Docker image.
    ![Docker run command](docker-run.png)
    4. After a successful docker run command, it will start a Docker container command prompt.
    ![Docker run result](docker-run-result.png)
    5. We can run tests using Docker containers. Below are the commands to run tests with pytest, along with screenshots. The main and test code can be found in the src/main/python and src/test folders, respectively.
    ![Pytest command](test1.png)
    6. Test result
    ![Pytest result](test-result.png)

### Push Docker image to Azure Container Registry

    1. Log into  Azure Container Registry.
    2. Create Docker image to upload.
    ![Image tag](docker-tag.png)
    3. Push created docker image to Azure Container Registry.
    ![Push docker image](docker-push1.png)

### Deploy Spark job on Kubernetes Service

    1. Login to your Azure account using azure CLI
    2. Integrate Azure Kubernetes Service with Azure Container Registry.
    3. Service account creation and assigning required cluster role.
    ![Service creation1](create_service1.png)
    ![Service creation2](create_service2.png)
    4. Run a Spark job within the bin directory in Spark Home on AKS.
    ![spark_submit](spark_sub.png)
    5. Follow screenshot shows details about running pods.
    ![Running pods](get_pods.png)
    6. After succefull job submision you will get following messages.
    ![Success message](spark_success.png)
    7. Check pod after job submit.
    ![Last pod](final_pod.png)
