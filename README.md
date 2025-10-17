End-to-End Spam Classifier with MLOps CI/CDThis project demonstrates a complete MLOps workflow for building, training, and deploying a spam classification model. The entire process, from data versioning to automated deployment, is handled through a CI/CD pipeline.Workflow OverviewThe core of this project is its automated CI/CD pipeline, which triggers on every push to the main branch.Code Push: A developer pushes new code or changes to the GitHub repository.CI/CD Trigger: GitHub Actions automatically triggers the CI/CD workflow.Setup & Test: The environment is set up, dependencies are installed, and unit tests are run.DVC Pipeline: DVC pulls the versioned dataset from S3 and reproduces the ML pipeline (dvc repro) to train the model and generate metrics.Build & Push Docker Image: A Docker image containing the application is built and pushed to a private repository in Amazon ECR.Deploy: The final step involves pulling the latest Docker image from ECR and deploying it on an AWS EC2 instance.Tech StackModel & App: Python, Scikit-learn, FastAPIData & Pipeline Versioning: DVCCI/CD Automation: GitHub ActionsContainerization: DockerCloud & Deployment: AWSS3: For remote DVC storage.ECR: For storing the Docker container image.EC2: For hosting the deployed application.Local Setup and ExecutionTo run this project on your local machine, follow these steps:Clone the Repositorygit clone [https://github.com/Devendhake18/MLOPs-CICD-Assignment.git](https://github.com/Devendhake18/MLOPs-CICD-Assignment.git)
cd MLOps-CICD-Assignment
Create a Virtual Environmentpython -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
Install Dependenciespip install -r requirements.txt
Configure AWS CredentialsTo connect DVC with S3, you need to configure your AWS credentials.aws configure
# Enter your AWS Access Key ID, Secret Access Key, region, and default output format.
Pull DataDownload the versioned data and models from S3 using DVC.dvc pull
Run the ML PipelineReproduce the pipeline locally to ensure everything works.dvc repro
Application APIThe deployed application exposes a /predict endpoint to classify new text.RequestTo get a prediction, send a POST request with a JSON payload:curl -X POST http://<YOUR_EC2_PUBLIC_IP>/predict \
     -H "Content-Type: application/json" \
     -d '{"text": "Congratulations! You have won a free ticket. Claim now!"}'
ResponseThe API will return a JSON response with the classification:{
  "prediction": "spam" 
}
Project Structure.
├── .dvc/                   # DVC metadata and config
├── .github/workflows/      # GitHub Actions CI/CD workflow
├── data/                   # Data files (tracked by DVC)
├── models/                 # Trained models (tracked by DVC)
├── reports/                # Metrics and plots (tracked by DVC)
├── src/                    # Source code for the application and ML pipeline
├── app.py                  # FastAPI application entrypoint
├── Dockerfile              # Instructions to build the container image
├── dvc.yaml                # DVC pipeline definition
├── params.yaml             # Model parameters
└── requirements.txt        # Python dependencies
