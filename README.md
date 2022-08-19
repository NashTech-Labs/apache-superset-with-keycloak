# Deploy Apache-superset with existing Postgresql Instance

## Prerequisites

* **Keycloak server**
    * Keycloak Realm
    * Keycloak Client & broker configured with OIDC protocols.
    * [client_secret.json](./client_secret.json) 
* A Postgresql Instance deployed on same kubernetes cluster
* Helm installed in system & basic knowledge of Helm.
* Admin username & password of postgres instance
* Superset Database created in postgresql (If not then follow step 3)
* **Optionally** Cert-manager if you want to have SSL certificates on hostnames. 


## Usage

### 1. Clone the repo

```bash
git clone https://github.com/knoldus/apache-superset-with-keycloak.git
cd apache-superset-with-keycloak
```

### 2. Edit or Replace client_secret.json file with yours.

a) Open [client_secret.json](./client_secret.json)

b) Edit values for your client environment setup

c) Flatten JSON with this command `cat client_secret.json|tr -d "\n"`

### 3. Create or Edit apache-superset/custom-values.yaml file

**Replace placeholders with username, password and service name.**

a) Replace every USERNAME with admin username of postgres

b) Replace every PASSWORD with admin password of postgres

c) Replace every POSTGRES_SERVICE_NAME.NAMESPACE with service name & namespace of postgres

d) replace or add flatten [client_secret.json](./client_secret.json) file in the `.Values.extraSecrets.client_secret.json`

e) Replace YOUR_REALM_NAME with your Realm name.

### 4. Create database in postgresql **(If not created)**

```bash
kubectl port-forward service/<SERVICE_NAME_OF_POSTGRES> 5432
createdb -h 127.0.0.1 -p 5432 -U <USERNAME> superset
```


### 5. Deploy Apache superset


```bash
helm upgrade --install superset superset-keycloak -f superset-keycloak/keycloak-values.yaml --namespace superset --create-namespace
```