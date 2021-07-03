# Basic MongoDB Note:

##Install MongoDB on Ubuntu

->Run the following command to import the MongoDB public GPG key −
  
  `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10`
  
->Create a /etc/apt/sources.list.d/mongodb.list file using the following command.
 
 `wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -`
  
->However, if you receive an error indicating that gnupg is not installed, you can:
    --Install gnupg and its required libraries using the following command:

   `sudo apt-get install gnupg`

->Once installed, retry importing the key:

    `wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add - `
    
->Create a list file for MongoDB

`echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list`

->Now issue the following command to update the repository:

   `sudo apt-get update`
   
->Next install the MongoDB by using the following command :

   `sudo apt-get install -y mongodb-org`
   
->If you are unsure which init system your platform uses, run the following command:

  `ps --no-headers -o comm 1`
  
->Start MongoDB

`sudo service mongodb start`
`sudo systemctl start mongod`
`sudo systemctl daemon-reload`

->Stop MongoDB

`sudo service mongodb stop`

->Restart MongoDB

`sudo service mongodb restart`

->To use MongoDB run the following command.
`mongo`

## Common Command

`show dbs # <=> Print a list of all databases on the server.
use <db> # <=> Switch current database to <db>
show collections # <=> Print a list of all collections for current database.
db.people.insert({name:"Ashwin",gender:"m"}) # <=> Inserts a new "person" collection with a new object in that collection
db.people.find(<any attributes or none>) # <=> Find all persons in the collection
db.people.update({name:"Ashwin"},{gender:"f"}) # <=> Updates the selected (1st parameter) person with new value(2nd parameter)
db.people.remove({name:"Ashwin"}) # <=> Remove where name is "Ashwin"
db.people.drop() # <=> Delete the "people" collection`

