# Module Planning pour Ignition

## Menu

* [Requêtes basiques](#requêtes-basiques-⬆️)
  - [Creation des tables SQL](#creation-des-tables-sql-⬆️)
    - [Utilisation de l'écriture des plannings en modbus](#utilisation-de-l'écriture-des-plannings-en-modbus-⬆️)
    - [Utilisation de l'écriture des plannings avec devIO](#utilisation-de-l'écriture-des-plannings-avec-devio-⬆️)
  - [Vider une table SQL](#vider-une-table-sql-⬆️)
  - [Supprimer une table SQL](#supprimer-une-table-sql-⬆️)
* [Requêtes avancées](#requêtes-avancées-⬆️)
  - [Equipement déjà présent dans la base de données](#equipement-déjà-présent-dans-la-base-de-données-⬆️)
    - [Ajouter un numéro modbus](#ajouter-un-numéro-modbus-⬆️)
    - [Ajouter un id et un nom devio](#ajouter-un-id-et-un-nom-devio-⬆️)

## Requêtes basiques [⬆️](#menu "Retour au menu")

ℹ️ Il est possible d'entrer des requêtes SQL via la fonctionnalité **Database Query Browser** *(dans Tools)* d'Ignition. Afin d'executer une requête SQL, il faut entrer la commande dans la zone à gauche du bouton *Execute* puis cliquer sur ce même bouton.

Dans cette rubrique, les requêtes pourront être simplement copier-coller et executer.

### Creation des tables SQL [⬆️](#menu "Retour au menu")

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

#### Utilisation de l'écriture des plannings en modbus [⬆️](#menu "Retour au menu")

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

#### Utilisation de l'écriture des plannings avec devIO [⬆️](#menu "Retour au menu")

```
ALTER TABLE equipements
ADD COLUMN id_devio INT,
ADD COLUMN nom_devio TEXT
```

### Vider une table SQL [⬆️](#menu "Retour au menu")

Pour vider une table sql il faudra entrer la commande `TRUNCATE TABLE nom_table` où le nom_table correspond au nom de la table à vider.

🚨 Attention : les données seront supprimées.

### Supprimer une table SQL [⬆️](#menu "Retour au menu")

Pour supprimer une table sql il faudra entrer la commande `DROP TABLE nom_table` où le nom_table correspond au nom de la table à vider.

🚨 Attention : les données seront supprimées.

## Requêtes avancées [⬆️](#menu "Retour au menu")

⚠️ Dans cette rubrique, il sera nécessaire de modifier certaines informations avant d'executer la requête : numero_modbus, nom_site, nom_groupe, nom_equipement, numero_id_devio, nom_devio.

### Equipement déjà présent dans la base de données [⬆️](#menu "Retour au menu")

#### Ajouter un numéro modbus [⬆️](#menu "Retour au menu")

```
UPDATE equipements 
SET 
  num_mb = numero_modbus
WHERE site = 'nom_site'
AND groupe = 'nom_groupe'
AND equipement = 'nom_equipement'
```

Si le site n'est pas présent dans la table devices, il faudra également entrer la commande suivante `Insert into devices (nom_device, nom_site) Values ('nom_device', 'nom_site')`

✨ *Une solution graphique sera proposé via une View Perspective d'Ignition.*

#### Ajouter un id et un nom devio [⬆️](#menu "Retour au menu")

```
UPDATE equipements 
SET 
  id_devio = numero_id_devio
  nom_devio = nom_devio
WHERE site = 'nom site'
AND groupe = 'nom groupe'
AND equipement = 'non_equipement'
```

✨ *Une solution graphique sera proposé via une View Perspective d'Ignition.*