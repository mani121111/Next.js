🧑‍💻 1. Source Code in GitHub
The Next.js project is stored in a GitHub repository.
Contains files like:
     package.json: dependencies and scripts (npm run build, npm start)
     .next/, pages/, app/: Next.js specific folders
     Dockerfile: to containerize the app
     Jenkinsfile: defines CI/CD pipeline
     k8s/: contains Kubernetes YAML files
✅ Git acts as the source of truth.

🔁 2. Jenkins CI/CD Pipeline

A Jenkins job is set up to automate the workflow.
Trigger: Manual or automatic (e.g., on push to main branch).
Jenkinsfile stages:
      Checkout Code – Pulls from GitHub.
      Install Dependencies – npm install
      Build Project – npm run build
      Dockerize App – docker build -t MynextJS .
      Push Image – Pushes image to Docker Hub.
      (Optional): Deploy to Kubernetes via kubectl apply.
🐳 3. Dockerization
The Dockerfile packages the Next.js app for deployment.
📄 Example Dockerfile: 
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
✅ Now you have a containerized app, ready to run anywhere.
☁️ 4. Push Docker Image to Docker Hub
After building the image in Jenkins, it is pushed to Docker Hub:
docker push mani121/MynextJS:latest
📦 5. Kubernetes Deployment
Once the image is available, you deploy it to a Kubernetes cluster like EKS
🧪 6. Deploy to K8s
You apply both manifests using:
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
Use kubectl get pods, kubectl get svc to verify it's running.
📈 7. (Optional) Auto-Deploy via Jenkins
You can add a final stage in your Jenkinsfile to automatically deploy after pushing the image:
stage('Deploy to K8s') {
    steps {
        sh 'kubectl apply -f DeploymentFile.yaml'
        sh 'kubectl apply -f Service.yaml'
    }
}
✅ Jenkins connects to Kubernetes and deploys the latest image.
✅ Final Flow Diagram (for Presentation)
GitHub
  ↓
Jenkins (CI/CD)
  ↓
Docker Build → Docker Hub
  ↓
Kubernetes (Deployment + Service)
  ↓
Live App (via LoadBalancer)
