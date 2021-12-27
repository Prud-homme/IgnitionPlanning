# Module Planning pour Ignition

## Requêtes basiques

ℹ️ Il est possible d'entrer des requêtes SQL via la fonctionnalité **Database Query Browser** *(dans Tools)* d'Ignition. Afin d'executer une requête SQL, il faut entrer la commande dans la zone à gauche du bouton *Execute* puis cliquer sur ce même bouton.

Dans cette rubrique, les commandes pourront être simplement copier-coller.

### Creation des tables SQL

```
CREATE TABLE equipements
(
  site TEXT,
  groupe TEXT,
  equipement TEXT
)
```

```
CREATE TABLE plannings
(
  h_start TEXT,
  m_start TEXT,
  h_stop TEXT,
  m_stop TEXT,
  plage INT,
  jour INT,
  site TEXT,
  groupe TEXT,
  equipement TEXT
)
```

```
CREATE TABLE plannings_exception
(
  h_start TEXT,
  m_start TEXT,
  h_stop TEXT,
  m_stop TEXT,
  date_deb TEXT,
  date_fin TEXT,
  plage INT,
  site TEXT,
  groupe TEXT,
  equipement TEXT,
  commentaire TEXT
)
```

Pour utiliser l'écriture des plannings en modbus, entrer les commandes suivantes:

```
CREATE TABLE devices
(
    nom_device TEXT NOT NULL,
    nom_site TEXT NOT NULL
)
```

```
ALTER TABLE equipements
ADD COLUMN num_mb INT
```

Pour utiliser l'écriture des plannings avec devIO, entrer la commandes suivante:

```
ALTER TABLE equipements
ADD COLUMN id_devio INT,
ADD COLUMN nom_devio TEXT
```

### Vider une table SQL

Pour vider une table sql il faudra entrer la commande `TRUNCATE TABLE nom_table` où le nom_table correspond au nom de la table à vider.

🚨 Attention : les données seront supprimées.

### Supprimer une table SQL

Pour supprimer une table sql il faudra entrer la commande `DROP TABLE nom_table` où le nom_table correspond au nom de la table à vider.

🚨 Attention : les données seront supprimées.

## Commandes avancées

⚠️ Dans cette rubrique, il sera nécessaire de modifier certaines informations avant d'executer la requête : numero_modbus, nom_site, nom_groupe, nom_equipement, numero_id_devio, nom_devio.

### Equipement déjà présent dans la base de données

#### Ajouter un numéro modbus 

```
UPDATE equipements 
SET 
  num_mb = numero_modbus
WHERE site = 'nom_site'
AND groupe = 'nom_groupe'
AND equipement = 'nom_equipement'
```

Si le site n'est pas présent dans la table devices, il faudra également entrer la commande suivante `Insert into devices (nom_device, nom_site) Values ('nom_device', 'nom_site')`

#### Ajouter un id et un nom devio

```
UPDATE equipements 
SET 
  id_devio = numero_id_devio
  nom_devio = nom_devio
WHERE site = 'nom site'
AND groupe = 'nom groupe'
AND equipement = 'non_equipement'
```

### Equipement non présent dans la base de données

#### Ajouter l'équipement avec un numéro modbus 

```
...
```

#### Ajouter l'équipement avec un id et un nom devio

```
...
```
