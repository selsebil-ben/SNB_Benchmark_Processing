#we gonne see how to import SNB CSVs in CozoDB
 first u go to  : https://ldbcouncil.org/data-sets-surf-repository/snb-interactive-v1-datagen-v100
2. Download the file : social_network-sfx-CsvBasic-LongDateFormatter.tar.zst (x=0.1 || 0.3||1)
3. unzip it , you will find two folders static/ and dynamic/
Le dossier static/ contient les données qui ne changent pas (entités “statiques”) — des choses comme Tag, Place, Organisation. 

Le dossier dynamic/ contient les relations changeantes / les événements du graphe — knows (relations d’amitié), les messages, commentaires, les liens “hasCreator”, “replyOf” etc.

then, You hafta clone the below structure in a root folder : 
 1 data folder : select all the csv files from static and dynamic folder in SNB folder and put them in data folder

2 Scripts folder contains all the scripts to transform csv data into json objects ready to be imported in cozoDB ( have a look here ^^ : https://docs.cozodb.org/en/latest/nonscript.html)
3 output_json folder , this is the output directory in with will put the chunks returned by csv_to_json python scripts. it contains also the query_cozo file to run cozo queries, this file must be in the same location as the DB folder (see IMPORTANT section)

3 State folder to save the state of last edge id in every csv file (we have to generate ids because in csv edges don't have ids)

4 Venv Virtual envirement folder contains the basic packages
5 Requirements file just for embedded DB

6 finally the batch file to be runned 
/!\  IMPORTANT 

for every SNB SF, before running the batch file, u hafta :

- go to scripts/import_to_cozo.py and run the already commented code ( it's a code to create the schema of a simple PG in cozoDB and to create the indexes) then you comment it again and let only import code discommented. once you runned the client you get a new folder DB in the same directory (scripts folder)
- go to state/ edge-counter.txt , the text file must be empty at the begining

