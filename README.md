# Student Result Management System


## Features

- View a student's marks, total, percentage, and PASS/FAIL status.
- Handle an unknown roll number with HTTP 404.
- Health-check endpoint for deployment verification.
- PyTest automated tests.
- GitHub Actions workflow for tests, SonarQube analysis, Docker build, and container smoke test.
- Dockerfile runs the application as a non-root user.

## Project files

- `app.py` — Flask application
- `test_app.py` — PyTest test cases
- `requirements.txt` — Python dependencies
- `Dockerfile` — container image definition
- `sonar-project.properties` — SonarQube project configuration
- `.github/workflows/devops-ci.yml` — GitHub Actions workflow

## Run locally

Python 3.12 is recommended.

```bash

pip install -r requirements.txt
pytest -v
python app.py
```

Open these URLs:

- `http://127.0.0.1:5000/`
- `http://127.0.0.1:5000/results/101`
- `http://127.0.0.1:5000/results/103`
- `http://127.0.0.1:5000/results/999`
- `http://127.0.0.1:5000/health`


## GitHub Actions setup

1. Create a GitHub repository and push all project files to the `main` branch.
2. In **Settings → Secrets and variables → Actions**, add:
   - Secret `SONAR_TOKEN`: token created in SonarQube.
   - Variable `SONAR_HOST_URL`: URL reachable by the GitHub Actions runner, for example `https://sonarqube.example.edu`.
3. Ensure the SonarQube project key `student-result-management` exists or that the token has permission to create/analyze the project.
4. Push a commit or open a pull request to trigger the workflow.
5. Review the Actions run. The Docker job builds and smoke-tests a container on the temporary GitHub-hosted runner.

**Network note:** A GitHub-hosted runner cannot reach a SonarQube server running only on `localhost` or a private lab network. Use a reachable SonarQube server, SonarQube Cloud with the appropriate configuration, or a self-hosted GitHub Actions runner in the lab network.

**Deployment note:** The workflow's Docker job is a deployment-style smoke test on an ephemeral runner; the container is removed when the job finishes. To deploy persistently, run the Docker commands on the target server or add a separately configured deployment job for the college's hosting environment. Do not store passwords or tokens in source code.

