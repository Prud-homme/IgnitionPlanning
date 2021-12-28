# Module Planning pour Ignition

## Menu

* [Gestion des équipements](#gestion-des-équipements)
  - [Description](#description)
    - [Initialisation](#initialisation)
    - [Mise à jour des tables](#mise-à-jour-des-tables)
  - [Utilisation](#utilisation)
* [Requêtes basiques](#requêtes-basiques)
  - [Creation des tables SQL](#creation-des-tables-sql)
    - [Utilisation de l'écriture des plannings en modbus](#utilisation-de-l'écriture-des-plannings-en-modbus)
    - [Utilisation de l'écriture des plannings avec devIO](#utilisation-de-l'écriture-des-plannings-avec-devio)
  - [Vider une table SQL](#vider-une-table-sql)
  - [Supprimer une table SQL](#supprimer-une-table-sql)
* [Requêtes avancées](#requêtes-avancées)
  - [Equipement déjà présent dans la base de données](#Equipement-déjà-présent-dans-la-base-de-données)
    - [Ajouter un numéro modbus](#ajouter-un-numéro-modbus)
    - [Ajouter un id et un nom devio](#ajouter-un-id-et-un-nom-devio)
* [Ajouter un équipement à la base de données](#ajouter-un-équipement-à-la-base-de-données)

ℹ️ Les parties Requêtes basiques et avancées sont présente à titre d'informations.

## Gestion des équipements

<div style="text-align:center">
<img src="https://raw.githubusercontent.com/Prud-homme/image-data-bank/main/HTTP/overview.png" alt="SQL_Planning" width="450" height="auto" />
</div>

✨ La view Utils/SQL_Planning permet de :
* créer les tables nécessaire à l'utilisation du module Planning
* activer le support modbus ou devio
* ajouter un équipement à la base de données
* ajouter une association entre un device et un site à la base de données
* ne pas utiliser le *Database Query Browser*

### Description

* *database* (CUSTOM) : nom de la base de données qui contient les tables *equipements, plannings* et *plannings_exception*
* le bouton _**Rafrîchir les tables**_ permet de synchroniser le premier tableau avec la table *equipements* et le second tableau avec la table *devices*

#### Initialisation

⚠️ Ne cliquer pas sur un des 3 boutons d'initialisation si la base de données contient déjà ce que l'on souhaite.

* _**Créer les tables**_ : création des tables *equipements, plannings* et *plannings_exception* dans la base données (nécessaire à l'utilisation du module Planning)
* _**Support Modbus**_ : ajoute la colonne *num_mb* à la table *equipements* et création de la table *devices* associant le nom d'un device au nom d'un site de la base de données (nécessaire pour écrire les plannings via Modbus)
* _**Support DevIO**_ : ajoute les colonnes *id_devio* et *nom_devio* à la table *equipements* (nécessaire pour écrire les plannings via DevIO)

#### Mise à jour des tables

* _**Inclure Numero Modbus**_ : option à cocher si l'on souhaite inserer les numero Modbus
* _**Inclure Id et Nom DevIO**_ : option à cocher si l'on souhaite inserer id_devio et nom_devio
* _**Mettre à jour equipements et plannings**_ : insertion des équipements présent dans le tableau situé au dessus du bouton dans les tables *equipements* et *plannings*
* _**Mettre à jour devices**_ : insertion des éléments du tableau dans la table *devices*


### Utilisation

ℹ️ Une fois la view ouverte avec le designer, passer en preview mode (*Play*) afin de pouvoir effectuer des cliques sur les boutons.

⚠️ Si les tables *equipements, plannings, plannings_exception* ne sont pas présente, il faut les créer via le bouton *Créer les tables* avant toute autre action. 

Si l'équipement est déjà présent, il ne sera pas de nouveau ajouter à la base de données et ainsi empêche l'apparition de doublons. Mais, il sera bien mise à jour si on lui ajoute un *num_mb*, *id_devio* ou *nom_devio*.

Rafraichir les tables supprime toutes les modifications effectuées sur les tableaux qui n'ont pas été enregistré via les boutons *mise à jour*.

Si l'écriture des plannings se fait via Modbus, que la colonne *num_mb* n'est pas présente dans la table *equipements* et que la table *devices* n'existe pas, il faut cliquer sur le bouton *Support Modbus*. Cliquer ensuite sur le bouton *Rafraichir les tables*.

Si l'écriture des plannings se fait via DevIO, que les colonnes *id_devio* et *nom_devio* ne sont pas présente dans la table *equipements*, il faut cliquer sur le bouton *Support DevIO*. Cliquer ensuite sur le bouton *Rafraichir les tables*.

Afin d'ajouter ou mettre à jour un equipement, il faudra éditer le dataset du premier tableau. *(PROPS data)*

Afin d'associer un device à un site de la base de données dans le cadre de l'écriture Modbus, il faudra éditer le dataset du second tableau. *(PROPS data)*


## Requêtes basiques

ℹ️ Il est possible d'entrer des requêtes SQL via la fonctionnalité **Database Query Browser** *(dans Tools)* d'Ignition. Afin d'executer une requête SQL, il faut entrer la commande dans la zone à gauche du bouton *Execute* puis cliquer sur ce même bouton.

Dans cette rubrique, les requêtes pourront être simplement copier-coller et executer.

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

#### Utilisation de l'écriture des plannings en modbus

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

#### Utilisation de l'écriture des plannings avec devIO

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

## Requêtes avancées

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

Si le site n'est pas présent dans la table devices, il faudra également entrer la commande suivante : 
```
Insert into devices (nom_device, nom_site) Values ('nom_device', 'nom_site')
```

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