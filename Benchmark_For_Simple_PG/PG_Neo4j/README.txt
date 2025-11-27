1. GO TO : https://ldbcouncil.org/data-sets-surf-repository/snb-interactive-v1-datagen-v100
2. Download the file : social_network-sfx-CsvBasic-LongDateFormatter.tar.zst (x=0.1 || 0.3||1)
3. unzip it , you will find two folders static/ and dynamic/
Le dossier static/ contient les données qui ne changent pas (entités “statiques”) — des choses comme Tag, Place, Organisation. 

Le dossier dynamic/ contient les relations changeantes / les événements du graphe — knows (relations d’amitié), les messages, commentaires, les liens “hasCreator”, “replyOf” etc.

PS : 
-All THE BELOW STEPS ARE AUTOMATED ( run the ps file named automated_import_fromSNB_to_Neo4j)
- Read IMPORTANT section before you run the ps file

4. select only csv files from both static/ and dynamic/ folders, than :
5. Préparer Neo4j : crer une DB et placer les CSV dans le dossier import
6. Créer les contraintes / index  its très important avant l’import !!!
exemple : 
CREATE CONSTRAINT person_id IF NOT EXISTS FOR (p:Person) REQUIRE p.id IS UNIQUE;

7.importer les noeuds 
exemple : 
USING PERIODIC COMMIT 5000
LOAD CSV WITH HEADERS FROM 'file:///person_0_0.csv' AS row
MERGE (p:Person {id: toInteger(row.id)})
SET p.firstName = row.firstName,
    p.lastName = row.lastName,
    p.gender = row.gender,
    p.birthday = row.birthday,      // si timestamp non-ISO, laisse en string
    p.city = row.city,
    p.country = row.country;

8. Importer les edges 
exemple :
USING PERIODIC COMMIT 5000
LOAD CSV WITH HEADERS FROM 'file:///knows_0_0.csv' AS row
MATCH (a:Person {id: toInteger(row.person1)}), (b:Person {id: toInteger(row.person2)})
CREATE (a)-[:KNOWS {creationDate: row.creationDate}]->(b);

/!\  IMPORTANT 

before running the ps file, u hafta :
- install the plugin APOC in your Neo4j DBMS
- go to dynamic and static folders and change the name of the first and second column in both person_knows_person, comment_replyOf_comment, place_isPartOf_place and tagclass_isSubclassOf_tagclass files ; the first column must be labelName1.id and the second column must be labelName2.id 
