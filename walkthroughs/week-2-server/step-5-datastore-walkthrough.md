# Datastore

## Getting Started

This walkthrough introduces Datastore, which lets you store data permanently.

You can return to this walkthrough anytime by running this command:

```bash
teachme step-5-datastore-walkthrough.md
```

Click the **Start** button to begin!

## The Goal

The goal of this project is to use Datastore to permanently store the comments
you implemented in week 2.

This walkthrough introduces a lot of concepts, but you don't have to use all of
them. Decide what makes sense for your goal.

Don't be afraid to get creative, but focus on finishing a
[minimum viable product](https://en.wikipedia.org/wiki/Minimum_viable_product)
that proves you have all of the pieces connected, and then come back later and
add extra features if you have time left over.

## Persistent Storage

So far, you've used a data structure like `ArrayList` to store comments in
memory. But what happens if you have more comments than fit in memory? What
happens when your server restarts, or when App Engine automatically scales your
server up or down?

The answer is that your data will be erased. This won't work for data that you
want to store permanently.

The solution to this problem is to use **persistent storage** to store the data
somewhere safer. You might use a database, or file storage.

This walkthrough introduces a tool called Datastore, which lets you store data
using a Java library.

## Datastore

[Datastore](https://cloud.google.com/appengine/docs/standard/java/datastore/) is
a [NoSQL](https://en.wikipedia.org/wiki/NoSQL) database that allows you to store
and load data using Java code.

The next few steps introduce Datastore through an example that stores tasks in a
todo list.

## Example

The `todo-list` directory contains an example project that uses Datastore to
store tasks posted by the user. The next few steps will walk you through this
example project.

## Maven Dependency

Datastore comes with the App Engine environment, so to use Datastore, first add
the App Engine dependency to your `pom.xml` file:

```xml
<dependency>
  <groupId>com.google.appengine</groupId>
  <artifactId>appengine-api-1.0-sdk</artifactId>
  <version>1.9.59</version>
</dependency>
```

This dependency allows you to reference the classes that come with the App
Engine SDK, which includes Datastore.

Add this dependency to the `pom.xml` file in your portfolio directory.

## DatastoreService

Open the `NewTaskServlet.java` file in the `todo-list` directory to see how it
uses Datastore to save a task.

To use Datastore, you first need an instance of the `DatastoreService` class.
You can call the `DatastoreServiceFactory.getDatastoreService()` function:

```java
DatastoreService datastore = DatastoreServiceFactory.getDatastoreService();
```

Now you can use this `datastore` variable to interact with Datastore.

You can read more about `DataStoreService`
[here](https://cloud.google.com/appengine/docs/standard/java/javadoc/com/google/appengine/api/datastore/DatastoreService.html).

## Entities

Data in Datastore is represented by **entities**. You can think of an entity
similar to how you think about an instance of a class in Java.

An entity has a **kind**, which is similar to a class name.

Look at the `doPost()` function in the `NewTaskServlet.java` file in the
`todo-list` directory to see how it creates an `Entity` by giving it a *kind* of
`Task`:

```java
Entity taskEntity = new Entity("Task");
```

This line of code creates an `Entity` with a *kind* of `Task` and stores it in a
`taskEntity` variable.

An entity also has **properties**, similar to how a class can have variables.
Each property is a **key** and a **value**. To set a property, you call the
`setProperty()` function:

```java
taskEntity.setProperty("title", title);
taskEntity.setProperty("timestamp", timestamp);
```

This code adds two properties to the `taskEntity` entity: `title` and
`timestamp`. Property values can be strings, numbers, or timestamps. See
[this guide](https://cloud.google.com/datastore/docs/concepts/entities#datastore-datastore-upsert-java)
to learn more about the types of values you can store in an entity.

### Storing Entities

Now that you have a `datastore` variable and a `taskEntity` variable, you can
store the entity by passing it into the `datastore.put()` function:

```java
datastore.put(taskEntity);
```

This function stores `taskEntity` in Datastore so that you can load it next time
you need it.

### Your Turn

Your first goal this week is to store comments as entities in Datastore. Get
this working before you worry about loading the data.

You can test this by running a dev server and then navigating to
[http://localhost:8080/_ah/admin](http://localhost:8080/_ah/admin) to view the
data in your local Datastore instance.

After you get this step working, create a pull request and send it to your
advisor for review!

## Loading Entities

Now that you have data stored in Datastore, you can load it whenever a user
requests it.

Read through the `doGet()` function of the `ListTasksServlet.java` file in the
`todo-list` directory to see an example of loading entities form Datastore.

First create a `Query` instance with the kind of entity you want to load, then
pass that `Query` into the `datastore.prepare()` function, which gives you a
`PreparedQuery` instance that contains all of the entities in Datastore with
that kind. To loop over those entities, call the `asIterable()` function.

```java
Query query = new Query("Task").addSort("timestamp", SortDirection.DESCENDING);
PreparedQuery results = datastore.prepare(query);
for (Entity entity : results.asIterable()) {
```

Then inside this loop, you can use the `entity.getProperty()` function to get
the properties that were set on each entity when it was stored in Datastore.

By storing entities when the user creates them and loading them when you need to
use them again, you can use Datastore as persistent storage even when your web
app is shut down or restarted.

### Your Turn

Next, add some code to your `DataServlet.java` file that loads comments from
Datastore.

Test that your code works by running a local dev server, adding a few comments,
and then restarting your server. Your comments should remain when you refresh
the page, and even when you restart your server!

## Admin Page

Datastore includes an admin page that gives you access to its data. This is very
useful for debugging and managing your data.

To view an admin page for your local dev server, go to
[http://localhost:8080/_ah/admin](http://localhost:8080/_ah/admin).

To view the admin page for your live server, go to
[https://console.cloud.google.com/datastore](https://console.cloud.google.com/datastore).

## Live Server

When you're happy with your feature and you're ready to show it to the world,
you can deploy it to your live server!

Your `appengine-web.xml` file should already contain your project ID. If so, you
can deploy to your live server by executing this command:

```bash
mvn appengine:update
```

After the command successfully completes, you can navigate to
`YOUR_PROJECT_ID.appspot.com` to see your portfolio and test your feature.

When you're done, share this link in the chat and with your team!

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

Congratulations! Your portfolio page should now support comments that are
permanently stored in Datastore.

### Learn More

This tutorial covered the fundamentals of how to use Datastore, but Datastore
also provides more advanced functionalities. See these resources for more info:

-   [Datastore documentation](https://cloud.google.com/datastore/docs/)
-   [Entities documentation](https://cloud.google.com/datastore/docs/concepts/entities)
-   [Datastore API](https://cloud.google.com/appengine/docs/standard/java/javadoc/com/google/appengine/api/datastore/package-summary)
