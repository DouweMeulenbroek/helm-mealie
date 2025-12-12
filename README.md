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
helm install mealie helm-mealie/mealie --create-namespace -n {{ Your Prefered Namespace }}
```

# FAQ
Q1. With traefik I get prompted for a basic auth login when first setting up mealie what do i do?
- If you set your own credentials for the basic auth in the values file you know the login and can use that to login to the basic auth prompt to access the admin pages. If you skip this part. `ingress.traefik.middleware.auth.credentials.username` and  `ingress.traefik.middleware.auth.credentials.password`
- If you did not provide a pasword or username yourself they will be created for you. The default username is `admin` and the password needs to be fetched from the secrets you created. These are stored as BASE64 encoded values in the secret 'mealie-adminpage-auth'. This can be fetched with

```bash
kubectl get secrets -n {{ your-mealie-namespace }} mealie-adminpage-auth -o yaml
```
Copy the BASE64 encoded value stored in data.password: and paste it in the following command to extract the password you can use for the basic-auth login prompt.
```bash
echo "{{ YOUR-BASE64-ENCODED-VALUE }}" | base64 --decode
```
Q2. After entering my basic-auth it redirects me back to the creation of an account again.
- I'm not sure what causes this. A reload of the page puts you towards the 'correct' home page or go back to your FQDN.

## Upgrade the release
If you want to upgrade values to suite newer configuration or update you can use the following command to upgrade your installation.
```bash
helm repo update
helm upgrade mealie mealie/mealie {{ Your Namespace }}
```

## Database upgrade
USE AT YOUR OWN RISK. I tested it on my setup but its not failproof. Use at own risk.

This helm chart now supports upgrading the database mayor versions. For this we unfortunately need to do several releases.

As always make sure you backup your mealie and postgresql/database PV.
This upgrade has been tested with chart version 1.0.6 to 1.1.0with postgresql moving from chart 15.5.38 to 16.7.5.

1. Set the following values postgresql.upgrade.temp_storage to `true` and run a helm upgrade.
```bash
helm repo update
helm upgrade mealie helm-mealie/mealie -n {{ your namesapce }}
```
Then wait till its done and then change the following value to true: postgresql.`upgrade.database.true`.
```bash
helm upgrade mealie helm-mealie/mealie -n {{ your namesapce }}
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
- Upgrade postgresql database version for local storage

# Features to be implemented
- LDAP integrated in the HELM-charts as subchart.
- Hardening recommendations.
- Fail2Ban
- Upgrade jobs cleanup
- ∞ Proper README.MD

# Troubleshooting
If the deployment of the statefulset is stuck 'missing pvc' you can rerun the upgrade command and it should then fix the missing PVC.


