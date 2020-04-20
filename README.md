### docker-compose example file to start an ACS cluster with search-services shards using DB_ID ###

# Author: Mark Tunmer

# Version: 1.0

# Details: 

The yml file is using ACS 6.1.0 and search services 1.3.0.7. change them as necessary

It will start 16 containers including the following

6 ACS instances in a cluster (2 ACS instances for Share, 4 ACS instances for solr shard tracking)
2 Share instances
1 Postgresql database
4 search services instances for the 4 DB_ID shards
1 ImageMagick
1 LibreOffice
1 PDF-Renderer

The processes all use their default ports within their containers, but they are exposed through docker as follows

http://localhost:<port\>/share
http://localhost:<port\>/alfresco
http://localhost:<port\>/solr

Alfresco front end Ports: 8081 & 9091

Share Ports: 8080 & 9090

ACS tracker Ports: 8060, 8061, 8062, 8063

Solr shard Ports: 9000, 9001, 9002, 9003

Feel free to change them to whatever suits as long as they don't overlap

# NOTE!!! ActiveMQ has not been installed so the Messaging Subsystem has been disabled. #

# Setup:
- Create a local target directory and unpack the zip file to that directory. 
- This will be $HOME on the local disk for the containers, including any files created under ./data for the database, content store and solr shards
- Import the postman collection into Postman, ready to run later and create the shards

# Start:
- Open a command prompt in the target directory
- Start the containers with the following command: 

    docker-compose up

- This will pull down the images from docker-hub if they aren't already stored locally, then start containers.

- Once the images are started, log into Alfresco admin console and check that all 6 ACS nodes are in the cluster

- Run the Postman collection to create the shards. The raw URLs can be taken from the JSON and run manually or using cURL if Postman isn't available.

# Stop:

- Stop the containers without deleting them
    
    docker-compose stop

- Stop and delete the containers and the container network
    
    docker-compose down

# Restarting

- If the container were stopped using docker-compose stop, restart them using this command

    docker-compose start

# Caveat: The db, content store and solr indexes are stored locally in volumes so will persist across restarts, even with 'docker-compose down'
