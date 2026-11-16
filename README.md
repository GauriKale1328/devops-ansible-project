# DevOps Project: Online Bookstore Deployment with Ansible

## 1. Scenario
An online bookstore web application (Flask) served through an Nginx reverse proxy on a Linux server.
Manual setup is slow and error-prone, so the server configuration and deployment are automated with Ansible.

## 2. DevOps Lifecycle Mapping
| Stage | Activity in this project | Tool |
|---|---|---|
| Plan | Define features and tasks | GitHub Issues |
| Code | App + Ansible code in version control | Git, GitHub |
| Build | Install dependencies, package app | pip / apt |
| Test | Syntax check, lint, health check | ansible-playbook --syntax-check, ansible-lint |
| Release | Merge to main, tag versions | Git tags |
| Deploy | Automated configuration and deployment | Ansible |
| Operate | Services managed by systemd | systemd, Nginx |
| Monitor | Health endpoint and service status | curl /health, systemctl |

## 3. Workflow / Architecture
```
Developer -> GitHub (code) -> GitHub Actions (syntax check + lint)
                                   |
                          Ansible control node
                                   | (inventory.ini)
                                   v
                        Web server (target host)
              Client -> Nginx (port 80) -> Flask app (port 5000, systemd)
```

## 4. Practices Used
Version control, Infrastructure as Code, CI, automation, idempotency, handlers, templating, health checks.

## 5. Automation Requirements
Install packages, create service user, deploy code, template configs, manage services, verify deployment, safe to re-run.

## 6. Project Structure
```
inventory.ini     # hosts (the [web] group)
ansible.cfg       # Ansible settings
site.yml          # main playbook
files/app.py      # Flask application
templates/        # Jinja2 templates (nginx, systemd)
.github/workflows/ci.yml  # CI pipeline
screenshots/      # execution screenshots
```

## 7. How to Run
```bash
ansible-inventory --list
ansible web -m ping
ansible-playbook site.yml --syntax-check
ansible-playbook site.yml
ansible-playbook site.yml   # second run: changed=0 (idempotent)
curl http://localhost
```
