![buster](images/blockbuster.jpg)


# DataBusterSQL
<br/>

The goal of this project is to build a database to manage a blockbuster business.
The complete pipeline includes these main steps:

* 1 Clean the DataFrames (original csv)
* 2 Design the EER concept
* 3 Make the necessary changes in the tables to accomodate into the EER
* 4 Connect to mySQL through python. Create the DDBB and Load the clean DFs
* 5 Establish the PK-FK relations between table IDs through MySQL Workbench
* 6 Run Queries to extract relevant info from the DDBB

<br/>
-----
<br/>
<br/>





# 1. DataBusterSQL

The data cleaning of each original table is performed in separated jupyter notebooks stored in:
<br/>
<br/>
`notebooks/def/`
- cleanDF_pipeline_`rental`.ipynb
- cleanDF_pipeline_`old_HDD`.ipynb
- cleanDF_pipeline_`language`ipynb
- cleanDF_pipeline_`actor`.ipynb
- cleanDF_pipeline_`category`ipynb
- cleanDF_pipeline_`inventory`.ipynb
- cleanDF_pipeline_`film`.ipynb


After cleaning the csv files from NAs, duplicates or uninformative values, we have to consider the business needs and data availability to design an efficient EER of the DDBB. I propose the following EER structure:

# 2. DDBB EER


![EERbuster](images/blockbuster_EER.png)


The DDBB will be made by **3 main tables**: `film.csv`, `rental.csv` and `policy.csv` <br/>
and **9 child tables**  `language.csv`, `category.csv`, `special_features.csv`, `actor.csv`, `film_has_category.csv`, 
`store.csv`, `staff.csv`, `inventory.csv`, `client.csv`

Next, I will enumerate the columns of each **main table**,<br/> the **FK's** will relate to the **child** tables just referred.


## `film.csv`

Previous assumptions: The business rents VHS and they only include 1 language
The DDBB could be adapted to renting DVDs easily as the multiple languages could be stored in  `inventory.csv` as a new column associated to the specific film `copy_id`.

* `(PK)film_id`
* title
* description
* release_year
* length
* rating


## `rental.csv`

Previous assumptions: <br/>
If a client rents multiple films, we will generate multiple rental_ids <br/>
The store possess multiple copies of the same film so **film_id != copy_id**

* `(PK)rental_id`
* rental_date
* return_date
   * invoice - as calculation of [(return_date-rental_date) * business.rental_rate]<br/>

`FOREIGN KEYS:` <br/>
**1 to many:** <br/>
   * client_id
   * staff_id
   * store_id
   * copy_id


## `policy.csv`

Previous assumptions:  <br/>
The DDBB manages multiple stores located in different countries under different renting policies.

`film_id` and `store_id` are represented as a single column `copy_id` that refers to the physical copy of the film. The use of this columns will also facilitate to keep the inventory track.

`(PK)copy_id`

* rental_rate - The renting price of the film varies according to the store location and film popularity
* rental_duration - The max renting duration varies according to the store location and film popularity
* replacement_cost - The selling price of the film varies according to the store location


## Child tables

### `store.csv`

* `(PK)store_id`
* country
* address


### `staff.csv`

* `(PK)staff_id`
* name
* address
* salary
* store_id (FK)
* supervised_by

### `client.csv`

* `(PK)client_id`
* address
* rental_id (FK)


###  `inventory.csv`
* `(PK)copy_id`
* film_id (FK)
* store_id (FK)
* language_id (FK)
* status (rented/available)

----

## Child tables related to the film info

###  `language.csv`,   `category.csv`,   `film_has_category.csv`,   `actor.csv`,   `special_features.csv`


## LOAD DDBB sql file and make some queries

1. Create a schema called `blockbuster``
2. Import data form `blockbuster.sql` available in `sql` folder
3. Run the queries available in XXX file also available in `sql` folder.