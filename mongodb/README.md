# Session 2. NoSQL Databases - MongoDB

## 1.	Introduction

This session is intended to help the student to get started with MongoDB, a NoSQL (document-oriented) database management system.

## 2. Install MongoDB

### 2.1 Install a local MongoDB server with Docker

*NOTE: If you don't have Docker try installing it (instructions [here](../docker.md)). If you are not able to use Docker you can install a MongoDB server following [this instructions](https://docs.mongodb.com/manual/installation/) and jump to section 3.*

**NOTE: If you had trouble setting up your machine, you can do the lab using an online sandbox environment like [this](https://onecompiler.com/mongodb) or [this](https://codapi.org/mongodb/). In that case, you can directly jump to section 3.3. Note that these sandboxes do not maintain state between runs, so you will need to chain all commands together. You won't be able to do the last part (interacting with mongodb from python) with a sandbox**


<!--
Let's first setup a local directory to be shared with the Docker container. Let's create a "drcav" directory in your home directory. 

	mkdir $HOME/drcav


* /Users/YOUR_USER_NAME/drcav in Mac
* /home/YOUR_USER_NAME/drcav in Linux 
* C:\Users\YOUR_USER_NAME\drcav in Windows

On Mac and Linux you can check your home directory by typing "echo $HOME" in a terminal. On Windows you can type "echo %cd%" in a terminal.

Let's pull and run a MongoDB Docker container:

	docker run -it --name=mongo -v $HOME/drcav:/drcav -p 27017:27017 -d mongo
-->

On the Ubuntu or Mac terminal run the following to pull and run a MongoDB Docker container:

	docker run -it --name=mongo -p 27017:27017 -d mongo:4.4.20

If you cannot run the previous command because you already created the container just type:

	docker start mongo 

*NOTE: For other commands related to Docker see [this guide](../docker_userguide.md).* 

<!--
On Windows 10, if you are running Docker Desktop, use a typical Windows path, e.g.:

	docker run -it --name=mongo -v C:\Users\YOUR_USER_NAME\drcav:/drcav -p 27017:27017 -d mongo
-->

<!--
On Windows 10 Home (Docker Toolbox):

	docker run -it --name=mongo -v //c/Users/YOUR_USER_NAME/drcav:/drcav -p 27017:27017 -d mongo
-->

Check if the container is running this way:

	docker ps -a
<!--
If you enter a wrong command by mistake you can delete the container this way:

	docker stop mongo
	docker rm mongo
-->

## 3 MongoDB tutorial 

### 3.1 Connecting with a client: MongoDB shell and MongoDB Compass

MongoDB comes with a shell client. If you installed MongoDB with Docker you can execute it this way:

	docker exec -it mongo mongo

<!--
	docker exec -it mongo bash	

Then you should see the typical Docker prompt, something like this:

	root@813847d78b39:/# 

Let's now run the MongoDB shell:

	root@813847d78b39:/# mongo

-->

*NOTE: If you don't work over Docker you can run "mongo" directly from a terminal.*

We will work on the shell, but it will be convenient to install MongoDB Compass, a GUI. You can dowload it here [here](https://www.mongodb.com/download-center/compass). Using MongoDB Compass is optional. Once you have Compass installed you have to enter this connection URL:
	
	mongodb://localhost:27017


### 3.2. First commands 

From the MongoDB shell, the following will inform you about the current active database:

	> db

Let's create and activate a new database:

 	>use drcavdb

you can get info about the new database this way:

	> db.stats()

If you have MongoDB Compass you can check that the drcavdb database has been created.

###  3.3. Working with collections

The following creates a new collection with one document:

	> db.photos.insert({
	      "title" : "Photo1",
	      "dateCreated": ISODate("2020-02-01T00:00:00Z"), 
	      "coord" : {
	         "lat" : -73.9557413,
	         "long" : 40.7720266,
	         "height" : 1439.2
	      },
	      "comments" : [
	         {
	            "author" : "User1",
	            "comment" : "Nice photo"
	         },
	         {
	            "author" : "User2",
	            "comment" : "Like this one"
	         }
	      ]
	   }
	)

Let's add another document:

    > db.photos.insert({
	      "title" : "Photo2",
	      "dateCreated": ISODate("2020-02-02T00:00:00Z"), 
	      "coord" : {
	         "lat" : -73.9557413,
	         "long" : 40.7720266,
	         "height" : 1439.2
	      },
	      "comments" : [
	         {
	            "author" : "User1",
	            "comment" : "Nice"
	         }
	      ]
	   }
	)

If you have MongoDB Compass you can use it to visualize the inserted documents. You an also insert documents with Compass. Click the ADD DATA button and try the Add Document functionality adding this one:

	{
      "title" : "Photo3",
      "dateCreated":{"$date":"2020-02-03"},
      "coord" : {
         "lat" : -73.9557413,
         "long" : 40.7720266,
         "height" : 1439.2
      },
      "comments" : [
         {
            "author" : "User1",
            "comment" : "Nice"
         }
      ]
	}

Notice that dates ("dateCreated" field) work slightly different on Compass. 

Now try to add a document with a different structure;

    {
	      "title" : "Photo4"
	}


Notice that MongoDB doesn't care that the document has a different structure.

On the shell you can visualize all the documents this way:

	> db.photos.find()

On the shell you can remove all the documents this way:

	> db.photos.remove({})

Let's reinsert two documents at the same time with:

	> db.photos.insertMany([ 
	{
	  "title" : "Photo1",
	  "dateCreated": ISODate("2020-02-01T00:00:00Z"), 
	  "coord" : {
	     "lat" : -73.9557413,
	     "long" : 40.7720266,
	     "height" : 1439.2
	  },
	  "comments" : [
	     {
	        "author" : "User1",
	        "comment" : "Nice photo"
	     },
	     {
	        "author" : "User2",
	        "comment" : "Like this one"
	     }
	  ]
	},
	{
	     "title" : "Photo2",
	     "dateCreated": ISODate("2020-02-02T00:00:00Z"), 
	     "coord" : {
	        "lat" : -73.9557413,
	        "long" : 40.7720266,
	        "height" : 1439.2
	     },
	     "comments" : [
	        {
	           "author" : "User1",
	           "comment" : "Nice"
	        }
	     ]
	}
	])


### 3.4. Querying

On the shell you can query all documents and pretty print them this way:

     >db.photos.find().pretty()

You can find documents with title "Photo2" this way:

     >db.photos.find( { "title": "Photo2" } )

On Compass, you can do the same by typing { "title": "Photo2" } into the FILTER field.

You can apply boolean conditions:

	> db.photos.find(
	   {
	      $or: [
	         {"title": "Photo1"}, {"title": "Photo2"}
	      ]
	   }
	)

You can filter by date:

	> db.photos.find( { dateCreated: { $gt: ISODate('2000-06-22') } } )

And [(JavaScript) regular expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet):

	> db.photos.find({"title": /Photo/})

You can perform aggregate operations such as count:

	> db.photos.count()
	> db.photos.count({"title": /Photo/})

### 3.5. Geospatial data

The way we have specified geographic coordinates in the documents inserted above does not allow us to take advantage of MongoDB's geospatial query capabilities. Let's remove all the documents:

	> db.photos.remove({})

Now, let's insert a new photo with geolcation the following way:

	> db.photos.insert( {
	  	  "title" : "Photo1",
		  "dateCreated": ISODate("2020-02-01T00:00:00Z"), 
		  location: { type: "Point", coordinates: [ -73.97, 40.77 ] }
	} );

MongoDB geospatial queries can interpret geometry on a flat surface or a sphere. You need to create a geospatial index (2dsphere or 2d) before performing geospatial queries:

	> db.photos.createIndex( { location: "2dsphere" } )

*NOTE: For simple euclidean coordinate pairs use the 2d index. For more sophisticated GeoJSON objects or for a Earth-like sphere geometry (WGS 84) use 2dsphere.*

Now you can find documents that are at least 1000 meters from and at most 5000 meters from the specified point:

	> db.photos.find(
	{
	 location:
	   { $near:
	      {
	        $geometry: { type: "Point",  coordinates: [ -73.9667, 40.78 ] },
	        $minDistance: 1000,
	        $maxDistance: 5000
	      }
	   }
	}
	)

You also can find documents that match the a query filter, sorted in order of nearest to farthest to the specified GeoJSON point:

	> db.photos.aggregate( [
	   {
	      $geoNear: {
	         near: { type: "Point", coordinates: [ -73.9667, 40.78 ] },
	         spherical: true,
	         query: { "title": /Photo/ },
	         distanceField: "calcDistance"
	      }
	   }
	] )

### 3.6. Accessing MongoDB from Python code

Exit the MongoDB shell

	> exit

*NOTE: From now on it is assumed that we are in a Linux-type terminal, whether it is a native one, on a Mac or one within Windows' WSL. If you were working directly in a Windows terminal you can follow the steps but you will have to adapt some of the commands.*

Let's first create a local directory named "mongodbclient" in your home directory:

	cd $HOME
	mkdir mongodbclient 
	cd mongodbclient

Check the absolute location of the folder (this will help you to find things later).

	pwd 

The following assumes that you have Python installed (as happens by default in Ubuntu/macOS). You can check that with (from the Ubuntu/macOS terminal):

	python --version

In case Python it's not installed do the following:

	sudo apt-get update
	sudo apt-get install -y python
	sudo apt-get install -y python3-venv

It's recommended to work within a virtual environment (this way the python libraries will not be mixed with the ones in your system). It's as easy as doing (from within the mongodbclient folder):

	python3 -m venv myvenv
	source myvenv/bin/activate

Let's install the required python library for working with MongoDB: 

	pip install pymongo

Let's now create a program that will send requests to the MongoDB server (a client program). Edit a new file named MyMongoClient.py with a text editor. *In case you are working on Windows with WSL2 see ANNEX 1 and ANNEX 2 of [this document](../wsl.md) to know how to edit files*.

	import pymongo

	myclient = pymongo.MongoClient("mongodb://localhost:27017/")
	mydb = myclient["drcavdb"]
	print("You are successfully connected to MongoDB!")

Save the file within the "mongodbclient" folder. If you have problems locating the folder with the editor ask the instructor.

Go back into the terminal and run the program:
	
	python MyMongoClient.py

Let's extend our code to query the "photos" collection:

	import pymongo

	myclient = pymongo.MongoClient("mongodb://localhost:27017/")
	mydb = myclient["drcavdb"]
	mycol = mydb["photos"]
	myquery = { "title": "Photo1" }
	mydoc = mycol.find(myquery)
	for x in mydoc:
		print(x)

## 4. Lab assignment

### 4.1 Inserting data (4 points)

*NOTE: Make sure to activate the drcavdb database each time you reenter the shell, otherwise you will create things within the test database*

Remove all the documents from the photos collection. Then, 
within the photos collection, insert JSON documents that contain the following information (filename, title, description, width, height, datetime_taken, latitude, longitude, username):

	"photo1.jpg", "photo 1", "winter landscape 1", 600, 400, '2019-02-02 10:10:10', 41.38, 2.15, "user1"
	"photo2.jpg", "photo 2", "winter landscape 2", 600, 400, '2019-02-02 10:10:10', 41.35, 2.16, "user1"
	"photo3.jpg", "photo 3", "winter landscape 3", 600, 400, '2019-02-02 10:10:10', 40.35, 2.18, "user2"

Create a new collection "Users" and insert JSON documents with the following information (username, password, email):

	"user1", "1234", "user1@gmail.com"
	"user2", "1234", "user2@gmail.com"
	"user3", "5555", "user3@upc.edu"

**Write the commands in a text file named commands.txt that you will deliver to the professor.**

### 4.2 Querying (4 points)

Now, try yourself writing commands that will allow you to obtain the following results (**write the commands in the commands.txt text file**):

1. Show all users and their data

2. Show all users with a gmail address

3. Show how many users have password = "1234"

4. Show photos that were taken at most 5000 meters from Barcelona (lat 41.38, long 2.15).

### 4.3 Accessing MongoDB from Python code (2 points)

Write a small python application that prints the photos that were taken at most 5000 meters from Barcelona (lat 41.38, long 2.15).

You can copy the program to the end of the commands.txt text file.

*NOTE: In Python you need to wrap with double quotes (") all field names (e.g "$near")*

## 5. Delivery

Deliver commands.txt file through the proper section within http://atenea.upc.edu. 
