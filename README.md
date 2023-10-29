![buster](images/blockbuster.jpg)

# DataBusterSQL

The goal of this project is to build your a database to manage a blockbuster business.
To do so, we will need to:

* 1. Clean the csv DataFrames 
* 2. Desing the EER concept and translate it to SQL
* 3. Make the necessary changes in the tables to accomodate the EER
* 4. Load clean DFs, establish the relations between IDs
* 5. Run Queries to check the DDBB status


After cleaning the csv files from NAs, duplicates or uninformative values, we have to consider the business necesities and data availability to desgin an efficient EER of the DDBB.

I propose the following EER structure:

## DDBB EER

The DDBB will be made by **3 main tables**: `film.csv`, `rental.csv` and `policy.csv` <br/>
and **8 child tables**  `language.csv`, `category.csv`, `special_features.csv`, `actor.csv`, 
`store.csv`, `staff.csv`, `inventory.csv`, `client.csv`

Next, I will enumerate the columns of each **main table**,<br/> the **FK's** will relate to the **peripheral** tables referred above.


## `film.csv`

Previous assumptions: The business rents VHS and they only include 1 language

* `(PK)film_id`
* title
* description
* release_year
* length
* rating

`FOREIGN KEYS:` <br/>
**1 to many:** <br/>
   * language_id
   * category_id
   * special_features_id
   * actor_id


## `rental.csv`

Previous assumptions: <br/>
If a client rents multiple films, we will generate multiple rental_ids <br/>
The store possess multiple copies of the same film so **film_id != copy_id**

* `(PK)rental_id`
* rental_date
* return_date

`FOREIGN KEYS:` <br/>
**1 to many:** <br/>
   * client_id
   * staff_id
   * store_id
   * copy_id
   * invoice - as calculation of [(return_date-rental_date) * business.rental_rate]<br/>


   ## `policy.csv`

Previous assumptions:  <br/>
The DDBB manages multiple stores located in different countries under different renting policies.

The PK is made by two FKs!!!

* `(PK)film_id`
* `(PK)store_id` 

* rental_rate - The renting price of the film varies according to the store location and film popularity
* rental_duration - The max renting duration varies according to the store location and film popularity
* replacement_cost - The selling price of the film varies according to the store location


## Child tables

### `store.csv`

* `(PK)store_id`
* country
* address
* staff_id (FK)

### `staff.csv`

* `(PK)staff_id`
* name
* address
* salary
* supervised_by

### `client.csv`

* `(PK)client_id`
* address
* rental_id (FK)


###  `inventory.csv`
* `(PK)copy_id`
* film_id (FK)
* store_id (FK)
* status (rented/available)

----

## Child tables related to the film info

###  `language.csv`,   `category.csv`,   `actor.csv`,   `special_features.csv`