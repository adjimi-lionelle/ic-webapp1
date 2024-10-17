## **3) Partie 3 : Déploiement des différentes applications dans un cluster Kubernetes.**

En nous basant sur cette architecture logicielle, nous allons donner le type et le rôle de chacune des ressources (A…H) mentionnées dans cette architecture.

A : Service ic-webapp
  Type : NodePort
  Rôle : permet d'accéder à l'application web ic-webapp 

B : ic-webapp
  Type: Pod
   Rôle: Il exécute l'application ic-webapp et répond aux requêtes HTTP envoyées via le service (A) 

C : Service pour Odoo Web
Type : Service NodePort 
Rôle : Ce service expose l'interface web d'Odoo, permettant aux utilisateurs ou autres services d'accéder au module web d'Odoo déployé dans le pod correspondant (D)   

D : Pod pour Odoo Web
Type : Pod
Rôle : Ce pod exécute l'application web Odoo et répond aux requêtes HTTP envoyées via le service (C)

E : Service pour la base de données Odoo
Type : Service ClusterIp
Rôle : Ce service expose la base de données Odoo pour être accessible par le pod Odoo Web (D) ou d'autres services. Il permet la communication avec la base de données qui est contenue dans le pod (F).

F : Pod pour la base de données Odoo
Type : Pod
Rôle : Ce pod exécute le serveur de la base de données PostgreSQL pour Odoo. Il contient les données nécessaires à l'application Odoo et permet la gestion de la persistance des données.

G : Service pour PgAdmin
Type : Service NodePort
Rôle : Ce service expose l'interface PgAdmin, qui est un outil d'administration graphique pour PostgreSQL. Il permet d'accéder au pod PgAdmin (H) pour interagir avec la base de données Odoo (F).

H : Pod pour PgAdmin
Type : Pod
Rôle : Ce pod exécute l'application PgAdmin, une interface web pour la gestion des bases de.


## **b) Déploiement des différentes applications**

-  Création d'un espace de nom (namespace) :

Pour créer le namespace, exécutez la commande suivante :

```bash
kubectl apply -f ic-webapp/ic-webapp-namespace.yml
```

- Définition des volumes persistants (Persistent Volume - PV et Persistent Volume Claim - PVC) 

Pour Odoo : 

```bash
kubectl apply -f odoo/odoo-pv.yml
kubectl apply -f odoo/odoo-pvc.yml

```

Pour PgAdmin :

```bash
kubectl apply -f pgadmin/pgadmin-pv.yml
kubectl apply -f pgadmin/pgadmin-pvc.yml
```

- Définition des Secrets kubernetes et un configMap

Pour Odoo :

```bash
kubectl apply -f odoo/odoo-secret.yml
```

Pour pgAdmin : 


```bash
kubectl apply -f pgadmin/pgadmin-secret.yml
kubectl apply -f pgadmin/pgadmin-configmap.yml
```

- Déploiement de PostgreSQL (Base de données d'Odoo) 

```bash
kubectl apply -f odoo/postgres-clusterip.yml
kubectl apply -f odoo/postgres-deployment.yml
```

- Déploiement pour Odoo 

```bash
kubectl apply -f odoo/odoo-deployment.yml
kubectl apply -f odoo/odoo-nodeport.yml
```

-  Déploiement pour PgAdmin

```bash
kubectl apply -f pgadmin/pgadmin-deployment.yml
kubectl apply -f pgadmin/pgadmin-nodeport.yml
```

- Déploiement de l'application web ic-webapp

```bash
kubectl apply -f ic-webapp/ic-webapp-deployment.yml
kubectl apply -f ic-webapp/ic-webapp-nodeport.yml
```

Tests des applications
