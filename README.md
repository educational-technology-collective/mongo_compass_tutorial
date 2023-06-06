# mongo_compass_tutorial

## Description

This is a guide to installing MongoDB Compass and connecting to the Nellodee remote server. As well as a brief overview of some simple mongo queries using the Compass GUI.


## Installation 

Navigate to [Install MongoDB Compass](https://www.mongodb.com/docs/compass/master/install/) <br>
Choose the version corresponding to your operating system.

You shouldn't need to install MongoDB itself to connect to a remote instance of MongoDB but I will leave the link to install MongoDB on your local system here: [Install MongoDB](https://www.mongodb.com/docs/manual/installation/) <br>
If you want to run/test/use MongoDB on your local system, you will need to follow the steps on the link above and make sure you choose the <b>Community Edition</b> corresponding to your operating system.

## Starting Mongod Service on Nellodee

* First check to see if the mongod service is already running by using the following command in the terminal `service mongod status`
    ![Mongo Status Check Ubuntu](/assets/mongo_status_ubuntu.png)

* If the `Active` property says "active (running)" then you can skip to the next step.

* If the `Active` property says "inactive (dead)" then you can run the following command `sudo systemctl start mongod`

## Connecting MongoDB Compass to Nellodee

At this point you should have a working version of the MongoDB Compass GUI installed and opened and confirmed that the mongo service has started on nellodee.

Watch the video to see me walk through the steps to connect to the mongo service on nellodee.



https://github.com/educational-technology-collective/mongo_compass_tutorial/assets/112991266/56e1aeee-dc51-43c4-b80b-e294fb7250c9



1. Click `New Connection` button
2. Click `Advanced Connection Options` expansion button
3. Choose `Proxy/SSH` tab
4. Fill in all four fields: 
    - SSH Hostname: `nellodee.si.umich.edu`
    - SSH Port: `22`
    - SSH Username: `your umich uniqname`
    - SSH Password: `password associated with your uniqname`
5. Click `Save & Connect`
6. You will be prompted to choose a color and enter a name for your connection

At this point you should be connected to the running mongo service on nellodee and see a list of the databases stored in mongo on nellodee. If you are already familiar with MongoDB queries then you should be good to go. 

If not, I will link a cheat sheet to standard mongo queries to get you started.

[MongoDB Queries](https://www.mongodb.com/docs/compass/current/query/filter/)
