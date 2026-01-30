# ECS160-HW1 
#### _(Start date: ) Jan 24, 2026_
#### _(Due date: ) Feb 14, 2026_

### Total points: 10

## Learning objectives: 
Java reflection, annotations, dynamic proxies.

## Problem Statement: Persistence framework using Java reflection. 
The persistence framework should be able to load and store simple Java objects 
consisting of `String`, `int`, and `byte[]` type fields from and to a SQLite database, as well as support lazy
loaded remote proxies for `byte[]` type fields.

## Prelimiaries

1. Please read this entire document carefully before starting to work on the solution. 

2. Please install openjdk-11, Apache Maven, and Sqlite3 on your machine. For example, on Ubuntu - 
```
sudo apt update
sudo apt install -y openjdk-11-jdk maven sqlite3

```

2. The [handout](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT) contains the scaffolding code. Please clone this repo. Your solution must be based on this repo. You are not permitted to modify any of the 
scaffolding code, or change any method signatures already provided. If you feel you _must_ do this, please ask on Piazza. 


4. The homework assumes some familiarity with SQL. SQL fundamentals will be covered in the [discussion](../slides/SQL.pdf). Additionally, [here](https://www.youtube.com/watch?v=3s0lFtUrhSQ) is a short introduction. Particularly, we will only be using plain
INSERT queries with PRIMARY KEYs, and SELECT queries in this homework, no JOINS, UPDATES, etc. We will be
using an embedded [SQLite database](https://github.com/xerial/sqlite-jdbc) for our purposes. The database and the library necessary to interface with it is already added to your `pom.xml`.

5. The amount of code you're expected to write is ~180 lines. This is only an estimate based on our reference solution. However, if you find that you're writing significantly more or less lines of code, you're encouraged to discuss your solution with us to make sure you're not going down a wrong path.

## Extra Credit
First 5 submissions to get full points on Gradescope will get 2 additional points.

## Task Details
This assignment's goal is to develop a SQLite persistence framework that can persist and load objects of any type, and test it with objects of the `Post` class. 

This persistence framework will provide the following annotations. Please add these annotations under the `persistence/` directory in the provided files.

1. `@Persistable` - When persisting an object into the SQLite database, only 
the  fields annotated with `@Persistable` should be saved in the database.
2. `@PrimaryKey` - Denotes that this field is the primary key (or ID field) of the class. 
3. `@RemoteLazyLoad` - This annotation will indicate that the field is lazily loaded from a remote URL.

You should add your code to the following two methods in the `SQLiteDB` class.
1. `insertRow(Object obj)` - This method should iterate over all the fields in the `obj` annotated with the `@Persistable` annotation and save them in the database using an `INSERT` query. You can assume that only fields of types `String`, `int`,
and `byte[]` are present in `obj` and that the object contains one and only one field annotated with `@PrimaryKey`. You can
also assume that the field annotated with `@PrimaryKey` will always also be annotated as `@Persistable`.
    1. No special functionality needs to be implemented for fields tagged with `@RemoteLazyLoad` when saving records.
    2. Please feel free to refer to the non-reflection implementation provided in [`SQLiteDBReference`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/SQLiteDBReference.java) to see how to interface with the database using JDBC.
2. `loadRow(Object obj)` - This method will accept an object `obj` with the field annotated as `@PrimaryKey` populated
and load the corresponding row from the database into `obj`. You can assume that the `obj` will always have a `@PrimaryKey` annotated field and it will contain a value. You should gracefully end the application if the primary key
in `obj` is not found in the database.
    1. If the object contains any field annotated with `@RemoteLazyLoad`, you will return a Javassist dynamic proxy instead. This will act as a "remote proxy", and will intercept all method invocations. For each field annotated with `@RemoteLazyLoad1`, the remote proxy will either contain
    an URL to the remote resource, or the bytes represented the actual resource.
    2. You can assume that all fields annotated with `@RemoteLazyLoad` also have the `@Persistable` annotation and are always of type `byte[]`.
    2. For all fields annotated with `@RemoteLazyLoad`, you will perform the following special handling. You will first find its getter methods by appending `get` to the field name. For example, if the field name is `myField`, the getter name is `getMyField`. 
    3. When any of the getter methods is invoked, you will check if the corresponding field value is an URL, and if it is, you will fetch the remote resource from the URL and return the remote resource as an array of bytes.

Please feel free to use the helper functions in [`SQLiteDBHelper.java`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/SQLiteDBHelper.java). Please note that the code in `insertRow` and `loadRow` should not contain any explicit use of the `Post` class.

## Grading Rubric
- Code compiles: 1 point
- The `insertRow` method in `SQLiteDB` class functionality - 
  - Saves some of the post object information: 3 points
  - Contains any logic specific to the `Post` class - **deduct 3 points**
- The `loadRow` method in `SQLiteDB` class functionality - 
  - Loads some of the post object information: 3 points
  - Performs lazy loading of remote objects correctly and saves a proper JPEG file: 3 points.
  - Contains any logic specific to the `Post` class - **deduct 6 points**

Essentially, you cannot use any class-specific information inside the two methods. If you do, you won't get credit for that component. 

## Relevant background

### JDBC API
Java DataBase Connectivity (JDBC) API is how Java interfaces with SQL databases.
We provide sample reference code in the [`SQLiteDBReference.java`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/SQLiteDBReference.java) file. In particular, 
the `insertPost` and `getPostByCid` methods show how to save and load post objects
without using Java reflection. Since this code does not use reflection, it is
specific to the `Post` class, so you shouldn't use this code directly in your solution. If you do, you'll not get any points! This code is for reference only.

The main steps are - 
1. Create a simple SQL query string with placeholders for the parameters
2. Create a `PreparedStatement` object from the string, and update the values in the placeholders using the `setString(pos, val)`, `setInt(pos, val)`, and the `setBytes(pos, val)` methods.
3. Execute the `PreparedStatement` and obtain the `ResultSet`.
4. Iterate over the `ResultSet` to retrieve the returned rows.

If you wish to learn more, [here](https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html) is a good reference document. But the examples
provided in the [`SQLiteDBReference.java`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/SQLiteDBReference.java) class should be enough for this homework.


### Handout code

Please spend some time studying the handout code. 

The handout code runs in two "modes".

1. If you don't provide any runtime argument, it runs in "insert" mode. In this mode it reads the `output.csv` file and populates `Post` objects, and then invokes the `insertRow` 
method to persist each of them. 
2. If you provide it with a post id, it will load the corresponding Post object from the database.

As mentioned earlier, the handout also contains a "reference" implementation, in 
the [`SQLiteDBReference`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/SQLiteDBReference.java) class, which does not
use any reflection. This is for reference only and you should not use it in your solution. Helper methods, for example, to fetch a remote URL, are provided in the [`SQLiteDBHelper.java`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/SQLiteDBHelper.java) class.

Note that the handout code won't compile until you add the annotation definitions in [`Persistable.java`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/Persistable.java), [`PrimaryKey.java`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/PrimaryKey.java), and [`RemoteLazyLoad.java`](https://github.com/davsec-teaching/ECS160_HW1_HANDOUT/blob/main/src/main/java/com/hw1/persistence/RemoteLazyLoad.java).

## Submission
1. Please add a README.md which clearly discloses any AI usage. 
1. Note that the Academic Integrity and AI Policy specified in the course page apply to all homeworks.
1. The handout comes with a `./test.sh` script. The test cases of the script should pass to get full credit. **Do not** modify this script.
3. The submission will be performed on Gradescope. Please note that the points you get on gradescope are final **only if** you
haven't violated any of the requirements in this handout. For example, we will manually validate that there is no class-specific logic in the `insertRow` and `loadRow` methods, and if there is, we will deduct points manually.  

