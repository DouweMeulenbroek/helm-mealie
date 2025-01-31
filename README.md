# Mealie
Mealie is an application for hosting recipes. https://github.com/mealie-recipes/mealie/
I personally like cooking but always  loose my recipes. Combining my passion for homelabbing and cooking I came across this awesome application.

# Installation
Requirements:
- Longhorn 
- Traefik
- an domain which is used to serve mealie
- Roughly 10GB of storage to allocate
- Edit the values.yaml to suite your configuration.

## Installation command
```bash
helm repo add helm-mealie https://DouweMeulenbroek.github.io/helm-mealie
helm repo update
helm install mealie helm-mealie/mealie --create-namespace
```

## Upgrade
If you want to upgrade values to suite newer configuration or update you can use the following command to upgrade your installation.

```bash
helm upgrade mealie mealie/mealie
```

# Configuration
## Must change values
- .Values.config.base_url
- .Values.ingress.traefik.tls.secret
## Important values
In the values.yaml you can edit the configuration. 
The following are impactfull:
- .Values.ingress.Traefik.enabled and .Values.ingress.nginx
- .Values.postgresql.enabled 
- .Values.SMTP # To your own mail server

# Personal Implementation choices
I personally use longhorn and Traefik and endorse its usage.
My stack is ran either on my cloud nodes or personal homelab with 8 RPI's (and growing).
TLS certificates are handled by CertManager. 

Storage: Longhorn
Networking: Traefik
Certificates: Certmanager

# Implemented but UNTESTED
- LDAP
- NGINX Ingress
- Local storage

# Features to be implemented
- local storage.
- LDAP integrated in the HELM-charts as subchart.
- Hardening recommendations.
- Fail2Ban
- Proper README.MD

# Troubleshooting
If the deployment of the statefulset is stuck 'missing pvc' you can rerun the upgrade command and it should then fix the missing PVC.
