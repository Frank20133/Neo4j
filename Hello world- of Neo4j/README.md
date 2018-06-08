# Hello world of Neo4j
- It should be a maven project and add
```XML
    </dependency>
    <dependency>
    <groupId>org.neo4j</groupId>
    <artifactId>neo4j</artifactId>
    <version>$version of Neo4j</version> <!-- The version of Neo4j should be something like '3.4.0' -->
    </dependency>
```
   in `pom.xml` in advance.
## FLOW
- Relationship types can be created by using an `enum` like
```java
private static enum RelTypes implements RelationshipType {
    KNOWS;
}
```
- The next step is to start the database server. Note that if the directory given for the database doesn’t already exist, it will be created.
```JAVA
//It should be included in createDb()
graph = GraphDatabaseFactory().newEmbeddedDatabase(dbPath);
registerShutdownHook( graphDb ); // It is used to make sure the database can be shutdown normally in case of JVM dropped out innormally.
```
- The paradigm of registerShutdownHook
```java
private static void registerShutDownHook(final GraphDatabaseService graphDb){
    Runtime.getRuntime().addShutdomeHook(new Thread() {
        @Override
        public void run() {
            graphDb.shutdown();
        }
    });
}
```
- The common flow should be first createDb and then shut it down
```java
public static void main(String[] args){
    Demo demo = new Demo();
    demo.createDb();
    demo.shutDown();
}
```
- All operations have to be performed in a transaction.
```java
try(Transaction tx = graphDb.beginTx()) {
    //Database operations go here
    tx.success();
}
```
- If you want to delete a relationship of two nodes, make sure deleting the relationship first but nodes. This is to ensure
relationships always have a start node and an end node.
