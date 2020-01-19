.. title: Scala Mongo example with akka-http and akka-stream
.. slug: scala-mongo-example-with-akka-http-and-akka-stream
.. date: 2019-12-23 16:22:54 UTC+07:00
.. tags: akka, akka-http, docker, sbt, scala, scala example 
.. category: technical
.. link: 
.. description: In this tutorial we shall create and CRUD web application in Scala. We will use MongoDB Scala Driver to communicate to the mongo database. It is officially supported Scala driver for MongoDB. Along with that we shall create a Rest API using akka-http module and akka-streams.
.. type: text


Introduction
##################

In this tutorial we shall create and CRUD web application in Scala. We will use  `MongoDB Scala Driver <https://docs.mongodb.com/ecosystem/drivers/scala />`_ to communicate to the mongo database. It is officially supported Scala driver for MongoDB. Along with that we shall create a Rest API using `akka-http <https://doc.akka.io/docs/akka-http/current/index.html />`_ module and `akka-streams <https://doc.akka.io/docs/akka/current/stream/index.html />`_.


Prerequisites
##################


#. Scala version: 2.13.1
#. Database: Mongo:3.4.23-xenial


Other dependencies - 
#########################

{{%gist 00a05c5c70722529d717f5908bb1d462 %}}


About the service/project
##################################
Application should be able to do CRUD operations on the in mongodb document. This  application should also provide Rest apis to access and modify the data. So it is an employee management. We have employee details in a document called `employee`. We will create reset services to create, update and search the employee data.


Document Class
##################################

Employee document contains employee name and date of birth.

{{%gist d176859653575565b4870087c16cad22 %}}

Mongo Configuration
##################################

**Codec Registry**

Mongo documents required the codec information for marshal and un-marshal the data. This codecs are already written and available here. This repository have codec implementations for most of the available types. So first create the the java codec as below -
 
{{%gist a68ec9ac38a65c38b6449347350dcec1 %}}

In the above code `Employee` is our mongo document class with the following details. 
 

Document Class
##################################
Employee document contains employee name and date of birth.

{{%gist d176859653575565b4870087c16cad22 %}}
 
Database Details
##################################
 
 
`MongoClientSettings` is the companion object to configure the mongodb settings. 

{{%gist ec33914a81f8f71d129cc9a16acf1e5b %}}
 
 
In the above snippet; A Mongo client settings object is created that contains the configuration details for the connecting mongo database.
 

Collection Details
##################################
 
 
Collection details can be obtained from the database representation we get the previous step
 
{{%gist aff2861b3333375066064d92d0848d43 %}}

This above step required for every collection to get the data.
 
JSON Support
##################################
We need formatter to format the date type to and from JSON format. We used Spray JSON here for this. Spray JSON providing automatic to and from JSON marshalling/un-marshalling using an in-scope *spray-json* protocol.

{{%gist e1b4b26a815a05e97f286d145ccce28c %}}
 
Repository
##################################
Repository contains the create, update, delete and search commands for the employee document. 

{{%gist a98e92e6ec7f07794cbfeab48fae3d22 %}}
 
Actors to get the user data 
###################################################
Employee actor received messages to perform different operation on the employee document. Actor shall call the actual service and then send the responds to the sender.

{{%gist 601add3fae9b2f44a93803da840f62a2 %}}


 
Routing Configuration
###################################################



+-------------------------------------+-----------------------------------+
| GET /api/employee/create            |  To create employee data          |
+-------------------------------------+-----------------------------------+
|POST /api/employee/search            | To search employee data           |
+-------------------------------------+-----------------------------------+
|PUT /api/employee/update             |  To update employee data          |
+-------------------------------------+-----------------------------------+
| DELETE /api/employee/delete         | To delete user data               |
+-------------------------------------+-----------------------------------+

 
The routing code should look like this-


{{%gist 448b06903269273a7d6439726badad71 %}}

Web Server
##################################


This class is used to create http server and bind the endpoints with the http server. You can get more information in `my previous blog <http://bit.ly/scalaakkarest />`_.


Run Application
##################################


Start the mongodb docker container 


`docker-compose up -d`


Compile the code using below command


`sbt compile`


Run the application


`sbt run`


Now go to terminal, and run below `httpie <https://httpie.org/ />`_ commands 


Create Employee Data
##################################
.. thumbnail:: /images/20200119/create-employee-data.png
   :alt: create employee

Search Employee Data
##################################
.. thumbnail:: /images/20200119/searchEmployeeData.png
   :alt: search employee


Update Employee Data
##################################


Check current data in mongodb

.. thumbnail:: /images/20200119/currentDataInMongo.png
   :alt: data in database

Update the data by id 

.. thumbnail:: /images/20200119/updateDataById.png
   :alt: update data by id


Search data after update 

.. thumbnail:: /images/20200119/searchDataAfterUpdate.png
   :alt: search data in database
 
 
Delete Employee data
##################################



Search employee data

.. thumbnail:: /images/20200119/searchDeleteEmployeeData.png
   :alt: search data in database


Delete employee data using id


.. thumbnail:: /images/20200119/deleteEmployeeDataById.png
   :alt: delete data in database

Search data after delete

.. thumbnail:: /images/20200119/searchDataAfterDelete.png
   :alt: search data in database


Github links
##################################


`Full code is available here <http://bit.ly/scala-mongo-crud />`_ to to explore and fork. Feel free to do whatever you want. ;)


Conclusion
##################################


We have seen here how to use akka http and streams to create rest services in scala. You can get more information here - 
https://doc.akka.io/docs/akka-http/current/introduction.html
https://doc.akka.io/docs/akka/current/stream/index.html
https://docs.mongodb.com/ecosystem/drivers/scala/


