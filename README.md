# CI/CD Pipeline for Kubernetes Deployment

This CI/CD pipeline script automates the deployment of a Kubernetes application using Jenkins, Docker, SonarQube, Nexus Repository Manager, Helm, and datree for misconfiguration checks.

## Pipeline Overview

This pipeline consists of the following stages:

1. Sonar Quality Check: This stage uses Maven and SonarQube to perform quality checks on the source code.
2. Docker Build & Push to Nexus Repo: This stage builds a Docker image and pushes it to a Nexus repository.
3. Identifying Misconfigs Using datree in Helm Charts: This stage uses datree to identify misconfigurations in Helm charts.
4. Pushing the Helm Charts to Nexus: This stage packages and uploads the Helm charts to a Nexus repository.
5. Deploying Application on Kubernetes Cluster: This stage deploys the application on a Kubernetes cluster using Helm.

## Prerequisites

Before running this pipeline, make sure you have the following prerequisites:

- Jenkins installed and configured
- Docker installed on the Jenkins agent
- SonarQube server and access token
- Nexus Repository Manager and admin credentials
- Helm and datree CLI installed
- Access to the Kubernetes cluster and kubeconfig file

## Usage

To use this CI/CD pipeline for Kubernetes deployment, follow these steps:

1. Set up Jenkins and configure the necessary plugins.
2. Create a Jenkins job and define the pipeline script.
3. Replace the credentials and URLs in the pipeline script with your own values.
4. Configure the Jenkins job to trigger the pipeline on code changes or on a schedule.
5. Push the code changes to the source repository to trigger the pipeline.
6. Monitor the pipeline execution and check for any errors or failures.
7. After successful deployment, access and test the application on the Kubernetes cluster.

## Cleanup

To clean up the deployed resources, follow these steps:

1. Delete the Kubernetes resources using Helm:
   ```bash
   helm delete myjavaapp
   ```
2. Remove the Docker images from the Nexus repository.
3. Remove any created artifacts or temporary files.

## Contributing

If you find any issues or have suggestions for improvements, feel free to open an issue or submit a pull request to this repository.

## License

This project is licensed under the MIT License.

Feel free to customize this README file based on your specific CI/CD pipeline configuration and deployment process.

# Screenshots

<img width="960" alt="cicdpipeline" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/6eb3de85-02d1-4ab2-9887-30501ee5623c">

<br>

<img width="960" alt="dat2" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/a034af28-9c00-4d85-8161-b0b79ea9daa5">

<br>

<img width="960" alt="datree" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/2adc26f2-7696-450c-b9fd-b1c838493ca6">

<br>

<img width="960" alt="Screenshot_20230220_033114" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/99479665-1324-4bf3-9f04-eb1ec561d870">

<br>

<img width="960" alt="Screenshot_20230220_033225" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/8f0cb675-44dd-452a-ae29-568bbbf0d883">

<br>

<img width="960" alt="Screenshot_20230220_033527" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/fcd903b2-04cc-4e54-9b0c-fdf968be3208">

<br>

<img width="960" alt="sonarqube" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/97340e99-c150-4b98-ae7c-204a3ae366d0">

<br>

<img width="955" alt="sonarqubesuccess" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/75041bc3-1f9b-406b-a093-3262575ed795">

<br>

<img width="960" alt="successnexus" src="https://github.com/Simrankhott/CI-CD-project/assets/91006102/1411be99-64c0-47d5-a66a-7080e7b0348a">
