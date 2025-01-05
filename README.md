# BoardgameListingWebApp

## Description

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Amazon Web Services(AWS) EC2
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## How to Run

1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application
4. To use initial user data, use the following credentials.
  - username: bugs    |     password: bunny (user role)
  - username: daffy   |     password: duck  (manager role)
5. You can also sign-up as a new user and customize your role to play with the application! 😊



## Create Service Account, Role & Assign that role, And create a secret for Service Account and geenrate a Token


## Role: Defines a set of permissions that can be granted to users or service accounts within a specific namespace.

## Role Binding: Links a Role to subjects (like users or service accounts) within a namespace, granting them the permissions specified by the Role.

 1. **Creating Service Account**:

 ```bash 

apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps


 `````

2. **Create Role**:

`````bash

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups:
        - ""
        - apps
        - autoscaling
        - batch
        - extensions
        - policy
        - rbac.authorization.k8s.io
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - pods
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```````


3.**Bind the role to service account**

```bash 
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-rolebinding
  namespace: webapps 
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role 
subjects:
- namespace: webapps 
  kind: ServiceAccount
  name: jenkins 
````

4. **Generate token using service account in the namespace**:

````bash
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: myserviceaccount  #replace name of the service-account
````````

- kubectl get secrets -n <namespace-name>
- kubectl describe secret <secret-name> -n <namespace-name>
- copy the Token[is used to authenticate from jenkins to kubernetes] --->jenkins credentials    -->select as secret text and create 
- Take help of pipeline systax[withkubeConfig Configure kubernetes CLI]
- Get the kubernetes server endpoint & Clustername ---> cd ~/.kube -->ls-->cat config 
- Generate a pipeline syntax


**kubectl**
```bash

curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
`````
**awscli**

```bash

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
aws configure

````
**eksctl**

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/c /usr/local/bin
eksctl version
```


5. You Might Face the Below Issues and Solutions:

- Connection Refused Issue:

```bash 
The connection to the server 172.31.26.61:6443 was refused - did you specify the right host or port?

# Check the status of kubelet:
sudo systemctl status kubelet

# Restart kubelet if needed:
sudo systemctl restart kubelet

`````
- Check Kubernetes Components:

```bash
kubectl get pods -n webapps
kubectl get svc -n webapps
kubectl get nodes -n webapps
```

6. While pushing artifact to the nexus replace the url of maven-releases & snapshots 


````bash 
<distributionManagement>
        <repository>
            <id>maven-releases</id>
            <url>http://43.205.242.45:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>maven-snapshots</id>
            <url>http://43.205.242.45:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>
````

8. Email-notification 

```bash
specific port open 465  

Manage google acount -->  security --> two step verificaiton --> App passwords --> copy the password

Dashboard --> ManageJenkins --> system --> extended E-main notification

SMTP server - smtp.gamil.com

SMTP port - 465

Advanced  - use ssl ✅ --> credentials --> username [email] --> that we have generated --give ID

select the credentials 

screendown to Email notifcation ---> SMTP server [smtp.gmail.com] -->Advanced --> use SSL --> SMTP port 465

Athentication --> username [email] -->password [App passowords] ---save and apply 

````



