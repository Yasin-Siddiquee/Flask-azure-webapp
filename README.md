# Flask App Deployed on Azure Web Apps

This is a simple Flask application that is deployed to **Azure Web Apps** using **GitHub Actions** for Continuous Integration and Continuous Deployment (CI/CD).

The application has a single endpoint that returns a "Hello, Azure Web Apps!" message when accessed.

## Project URL
You can access the live app at: [https://my-flask-app.azurewebsites.net](https://my-flask-app.azurewebsites.net)

## Table of Contents
- [About](#about)
- [Technologies](#technologies)
- [Setup](#setup)
- [Deployment](#deployment)
- [GitHub Actions Workflow](#github-actions-workflow)

## About

This project demonstrates how to deploy a simple Flask app to Azure Web Apps with an automated CI/CD pipeline using GitHub Actions. The app runs on Python 3.x, and it is configured to run on port `8080`.

## Technologies

- **Flask**: A lightweight Python web framework.
- **Azure Web Apps**: Cloud platform to host web apps.
- **GitHub Actions**: CI/CD service to automate deployment.
- **Azure CLI**: Command-line tools for managing Azure resources.

## Setup

### Prerequisites

- Python 3.12 installed.
- Azure account and subscription.
- GitHub account with access to a repository.

### Local Development

To run the Flask app locally, follow these steps:

1. Clone the repository:
   ```bash
   git clone https://github.com/Yasin-Siddiquee/Flask-azure-webapp.git
   ```
2. Create a virtual environment and activate it:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Run the Flask app:
   ```bash
   python app.py
   ```
   The app will be available at `http://127.0.0.1:8080`.

### Application Structure
- `app.py:` Contains the Flask application code.

- `requirements.txt:` Lists the Python dependencies for the app.

- `.github/workflows/deploy.yml:` GitHub Actions workflow file for deploying the app to Azure.

## Deployment

### Azure Web Apps Setup
1. Create an Azure Web App via the Azure Portal or Azure CLI.

2. Set environment variable `WEBSITES_PORT` to `8080` in the Azure Web App settings.

3. Use the Publish Profile from Azure Web App for GitHub Actions CI/CD deployment.

### GitHub Actions CI/CD
The app is set up to use GitHub Actions for automatic deployment to Azure Web Apps on every push to the `main` branch.

The workflow is defined in `.github/workflows/deploy.yml` and contains the following steps:

- **Checkout code**: Downloads the code from GitHub.

- **Setup Python**: Sets up Python 3.x in the environment.

- **Install dependencies**: Installs Python packages from `requirements.txt`.

- **Deploy to Azure**: Uses `azure/webapps-deploy` action to deploy the app to Azure using the publish profile.

### Triggering Deployment
Simply push your changes to the main branch, and GitHub Actions will automatically deploy your updates to Azure Web Apps.
```bash
git add .
git commit -m "Your commit message"
git push origin main
```

### GitHub Actions Workflow

```bash
name: Flask Azure Web App Deploy

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3
      with:
        node-version: '20'

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        node-version: '20'
        python-version: '3.x'

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v2
      with:
        app-name: 'my-flask-app'
        slot-name: 'production'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
```



