# Postgres_DataManipulationExercise
postgres=# \dt
           List of relations
 Schema |   Name    | Type  |  Owner   
--------+-----------+-------+----------
 public | user_data | table | postgres
(1 row)

postgres=# \d user_data
        Table "public.user_data"
 Column |       Type        | Modifiers 
--------+-------------------+-----------
 name   | character varying | 
 state  | character(2)      | 

postgres=# \set AUTOCOMMIT off
postgres=# BEGIN;
BEGIN
postgres=# DELETE FROM "user_data" WHERE "state" IN ('NY', 'CA');
DELETE 7
postgres=# ALTER TABLE "user_data"
postgres-#   ADD COLUMN "first_name" VARCHAR,
postgres-#   ADD COLUMN "last_name" VARCHAR;
ALTER TABLE
postgres=# \d user_data
          Table "public.user_data"
   Column   |       Type        | Modifiers 
------------+-------------------+-----------
 name       | character varying | 
 state      | character(2)      | 
 first_name | character varying | 
 last_name  | character varying | 

postgres=# UPDATE "user_data" SET
postgres-#   "first_name" = SPLIT_PART("name", ' ', 1),
postgres-#   "last_name" = SPLIT_PART("name", ' ', 2);
UPDATE 93
postgres=# ALTER TABLE "user_data" DROP COLUMN "name";
ALTER TABLE
postgres=# \d user_data
          Table "public.user_data"
   Column   |       Type        | Modifiers 
------------+-------------------+-----------
 state      | character(2)      | 
 first_name | character varying | 
 last_name  | character varying | 

postgres=# SELECT * FROM user_data LIMIT 5;
 state | first_name | last_name 
-------+------------+-----------
 MO    | Winter     | Chambers
 VA    | Fredericka | Pugh
 UT    | Maxine     | Hood
 ID    | Meredith   | Vincent
 KY    | Katell     | Booker
(5 rows)

postgres=# CREATE TABLE "states" (
postgres(#   "id" SMALLSERIAL,
postgres(#   "state" CHAR(2)
postgres(# );
CREATE TABLE
postgres=# \d states
                            Table "public.states"
 Column |     Type     |                      Modifiers                      
--------+--------------+-----------------------------------------------------
 id     | smallint     | not null default nextval('states_id_seq'::regclass)
 state  | character(2) | 

postgres=# INSERT INTO "states" ("state")
postgres-#   SELECT DISTINCT "state" FROM "user_data";
INSERT 0 35
postgres=# SELECT * FROM states LIMIT 5;
 id | state 
----+-------
  1 | OR
  2 | IN
  3 | UT
  4 | MI
  5 | MN
(5 rows)

postgres=# ALTER TABLE "user_data" ADD COLUMN "state_id" SMALLINT;
ALTER TABLE
postgres=# UPDATE "user_data" SET "state_id" = (
postgres(#   SELECT "s"."id"
postgres(#   FROM "states" "s"
postgres(#   WHERE "s"."state" = "user_data"."state"
postgres(# );
UPDATE 93
postgres=# SELECT * FROM user_data LIMIT 5;
 state | first_name | last_name | state_id 
-------+------------+-----------+----------
 MO    | Winter     | Chambers  |       32
 VA    | Fredericka | Pugh      |       25
 UT    | Maxine     | Hood      |        3
 ID    | Meredith   | Vincent   |       35
 KY    | Katell     | Booker    |       24
(5 rows)

postgres=# ALTER TABLE "user_data" DROP COLUMN "state";
ALTER TABLE
postgres=# SELECT * FROM user_data LIMIT 5;
 first_name | last_name | state_id 
------------+-----------+----------
 Winter     | Chambers  |       32
 Fredericka | Pugh      |       25
 Maxine     | Hood      |        3
 Meredith   | Vincent   |       35
 Katell     | Booker    |       24
(5 rows)

postgres=# COMMIT;
COMMIT
postgres=# SELECT * FROM user_data;
 first_name | last_name  | state_id 
------------+------------+----------
 Winter     | Chambers   |       32
 Fredericka | Pugh       |       25
 Maxine     | Hood       |        3
 Meredith   | Vincent    |       35
 Katell     | Booker     |       24
 Hannah     | Nixon      |        6
 Maisie     | Alexander  |        3
 Ebony      | Schroeder  |        6
 Maite      | Daniels    |        3
 Alexa      | Nielsen    |       16
 Fallon     | Valdez     |       32
 Emerald    | Cooper     |       13
 Galena     | Garner     |        6
 Cruz       | Hodge      |       35
 Harriet    | Cardenas   |       15
 Jana       | Mccall     |       23
 Teegan     | Casey      |       34
 Karleigh   | Lamb       |       12
 Caesar     | Olson      |       27
 Amanda     | Tucker     |       26
 Kim        | Andrews    |       27
 Nero       | Bates      |        9
 Felix      | George     |       14
 Jameson    | Keller     |       26
 Amelia     | Dominguez  |       34
 Castor     | Humphrey   |       33
 Jelani     | Gibson     |        4
 Duncan     | Mercer     |       30
 Reuben     | Dean       |       22
 Ursula     | Martin     |       31
 Marny      | Villarreal |        8
 Herman     | Chapman    |        2
 Owen       | Good       |       15
 Audrey     | Emerson    |       16
 Tatyana    | Craig      |       22
 Karly      | Rosales    |       22
 Ignatius   | Beard      |       16
 Lillith    | Kelley     |       25
 Addison    | Oneill     |       30
 Geoffrey   | Carson     |        6
 Rudyard    | Saunders   |       28
 Demetria   | Wiggins    |       26
 Adrian     | Key        |        3
 Cairo      | Herman     |       11
 Karleigh   | Lane       |        2
 Thaddeus   | Mann       |       28
 Jolie      | Hale       |       33
 Jasper     | Leonard    |       12
 Armand     | Roman      |       25
 Shelby     | Riddle     |        2
 Stella     | Porter     |       14
 Reed       | Todd       |        5
 Faith      | Drake      |       18
 Sebastian  | Good       |       32
 Leslie     | Kent       |        6
 Shea       | Watkins    |       29
 Daryl      | Crawford   |       17
 Noelani    | Sweeney    |       24
 Amery      | Donaldson  |       21
 Malcolm    | Sanford    |       15
 Camilla    | Decker     |       20
 Boris      | Cannon     |       13
 Isaiah     | Sawyer     |        1
 Rose       | English    |       26
 Lael       | Roman      |        5
 Abel       | Salas      |        3
 Chantale   | Gutierrez  |        3
 Nyssa      | Adams      |       25
 Quail      | Hayes      |        7
 Rose       | Harmon     |       27
 Sawyer     | Hood       |       31
 Wyoming    | Wyatt      |        3
 Maya       | Wright     |        6
 Ayanna     | Goff       |       35
 Quin       | Gaines     |       16
 Sawyer     | Carson     |       19
 Malachi    | Salazar    |       17
 Myles      | Aguirre    |       29
 Lydia      | Burnett    |       28
 Lila       | Livingston |       10
 Sonia      | Bryan      |       30
 Upton      | Vaughn     |       22
 Kasper     | Randall    |       18
 Kameko     | Kane       |       12
 Maisie     | Sanchez    |        7
 Cruz       | Miles      |       25
 Sophia     | Callahan   |        6
 Jemima     | Key        |       23
 Stephen    | Jennings   |       10
 Montana    | Mcclain    |       31
 Boris      | Wilcox     |       23
 Valentine  | Caldwell   |        7
 Reuben     | Slater     |        2
(93 rows)
