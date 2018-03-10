---
id: 762
title: Introduction to Room for Android
date: 2018-02-17T08:00:46+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=762
permalink: /2018/02/introduction-to-room-for-android/
categories:
  - Android
tags:
  - Android
  - App
  - Development
  - Programming
---
In the context of I/O 2017, Google released the Room Persistence Library, an intermediate layer between application logic and Android&#8217;s SQLite database. Using Room, you no longer have to handle stuff like network connection errors or caching yourself. Today&#8217;s article will help you get started with the library and teach you all the skills you need to implement Room in your own applications.

## Background of my sample application

The following code snippets are part of one of my side projects. This application manages shopping lists, which in itself can have entries. Actually, this is a very simple application, but it illustrates the functionality of Room very well.

## Room Components

The structure of Room is based on three elements, which I will explain in detail below. These elements are the entities, data access objects and the database itself. While the purpose of the database should be clear, I would like to comment briefly on that of the other two.

### Entities

Entities in general represent real things. Almost anything can be an entity. Think of a car, a house, yourself.

I wanted to keep the application very minimalistic, which is why each shopping list only has a title and a timestamp. This allows the user to sort the list chronologically. The class for displaying entries has the same attributes, supplemented by a reference to a shopping list. This is already a bit strange, because this setup corresponds to that of a relational database. When working with object relational mappers, however, you would rather give each shopping list a list of entry elements directly. Room separates itself from this approach: Room doesn&#8217;t allow object references. Google covers this extensively in a [dedicated article](https://developer.android.com/training/data-storage/room/referencing-data.html#understand-no-object-references). Instead, you have to explicitly tell Room what parts you need.

For this reason I will build my pojos according to a relational database schema. This results in the following schema:

<img src="https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2018/02/ShoppingListRoomERM.png?resize=750%2C139&#038;ssl=1" alt="ShoppingList Room ERM" class="aligncenter wp-image-768 size-full" width="750" height="139" srcset="https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2018/02/ShoppingListRoomERM.png?w=802&ssl=1 802w, https://i2.wp.com/blog.mjurtz.com/wp-content/uploads/2018/02/ShoppingListRoomERM.png?resize=500%2C93&ssl=1 500w" sizes="(max-width: 750px) 100vw, 750px" data-recalc-dims="1" />

In Android, this schema can be translated to regular POJO classes. Using Java annotations, you can tell your class what elements should be treated in which way. The class which represents a shopping list looks like this:

<pre class="EnlighterJSRAW" data-enlighter-language="java">@Entity(tableName = "shopping_list")
public class ShoppingList {

    @PrimaryKey(autoGenerate = true)
    public int id;

    public String description;

    @ColumnInfo(name = "timestamp_seconds")
    public int timestampSeconds;

    @Ignore
    public List&lt;ShoppingListItem&gt; items;
}</pre>

As you can see, there are already multiple annotations. The first one is the _@Entity_ annotation. It tells room that this class needs to be persisted to a database and what the name of the matching table will be. The next annotation is @PrimaryKey. You need to assign a primary key to each entity. Doing so, you specify a unique identifier to each of the tables entries. I added _autoGenerate = true_, so Room will be handling this matter for me.

In general, I don&#8217;t recommend to use simple integers as primary keys, but that&#8217;s another topic. Also, for the sake of simplicity, I set the scope of all properties to public. When using _private_, Room requires appropriate accessor methods to these properties.

Per default, room will use class and property names to specify the respective elements in the database. By using the _@ColumnInfo_ annotation, you can customize these. The last annotation in this example is _@Ignore_, which tells room to ignore the property when loading or saving data.

You can see another annotation when checking out the class representing an item inside a shopping list:

<pre class="EnlighterJSRAW" data-enlighter-language="java">@Entity(
        tableName = "shopping_list_item",
        foreignKeys = @ForeignKey(
                entity = ShoppingList.class,
                parentColumns = "id",
                childColumns = "list_id",
                onDelete=CASCADE))
public class ShoppingListItem {

    @PrimaryKey(autoGenerate = true)
    @ColumnInfo(name = "id")
    public int id;

    @ColumnInfo(name = "list_id")
    public int listId;

    @ColumnInfo(name = "description")
    public String description;

    @ColumnInfo(name = "timestamp_seconds")
    public int timestampSeconds;
}</pre>

As mentioned earlier, I&#8217;ve added a reference to the shopping list to each item. Since the id we specified for these is an automatically generate integer, we can simply access it here and store it as integer as well. By adding a _@ForeignKey_ annotation, you&#8217;ll firstly need to specify the referenced class, followed by the referenced row. Complete the statement by specifying the column of the current class with which the join is to be done. In my case, this is the _id_ column of the ShoppingList class and the _list_id_ column of the ShoppingListItem class. Note that the names must match those of the table and you may have overwritten them with _@ColumnInfo_ annotation.

You can also customize the OnDelete action. If CASCADE is specified as an OnDelete action, all items in a list are deleted if the list itself is deleted.

### Data Access Objects

The next part we will cover are the DAOs. Data Access Objects allow you to interact between the database and the entity. And here&#8217;s the best part: they are very easy to implement. I have created two DAOs, one for the lists and one for the entries. In order to keep the whole thing a little bit short here, I&#8217;m just going to go into the DAO of my list.

<pre class="EnlighterJSRAW" data-enlighter-language="null">@Dao
public interface ShoppingListDao {

    @Query("SELECT * FROM shopping_list")
    List&lt;ShoppingList&gt; getAll();

    @Query("DELETE FROM shopping_list WHERE id = :id")
    void deleteListById(int id);

    @Insert()
    void insertList(ShoppingList shoppingList);

    @Update()
    void updateList(ShoppingList shoppingList);

    @Delete
    void deleteList(ShoppingList shoppingList);
}</pre>

A DAO is basically an interface that describes the interaction possibilities with the database. How exactly these look depends of course on your specific application. By specifying a Java annotation you can control the type of operation. In addition to the regular operations for creating, updating, or deleting objects, Room offers you the option of implementing your own queries using the query statement.

Of course, you can also use parameters in your custom queries. To do this, simply enter the name of the parameter with a preceding colon in the query.

In my final section, the database itself, I will show you how to combine the components and how to persist your objects.

### Database

To interact with the SQLite database, you need to create a separate class that inherits from _RoomDatabase_ and passes the connection details via _@Database_ annotation. Google recommends using a singleton pattern, which I have implemented in my application.

<pre class="EnlighterJSRAW" data-enlighter-language="java">@Database(entities = {ShoppingList.class, ShoppingListItem.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {

    private static AppDatabase INSTANCE;

    public abstract ShoppingListDao shoppingListDao();
    public abstract ShoppingListItemDao shoppingListItemDao();

    public static AppDatabase getAppDatabase(Context context) {
        if (INSTANCE == null) {
            INSTANCE =
                    Room.databaseBuilder(context.getApplicationContext(), AppDatabase.class, "shoppinglist-db")
                            .allowMainThreadQueries()
                            .build();
        }
        return INSTANCE;
    }

    public static void destroyInstance() {
        INSTANCE = null;
    }
}</pre>

As you may have noticed, I explicitly allowed Room to perform database operations in the UI thread. Please note you should not release something like this in a production-ready application. Otherwise, longer running operations may block the user interface and have negative effects on the UX.

The following example shows you how to retrieve data from the database.

<pre class="EnlighterJSRAW" data-enlighter-language="java">List&lt;ShoppingList&gt; shoppingLists = AppDatabase.getAppDatabase(getApplicationContext()).shoppingListDao().getAll();</pre>

You can now perform all other operations in the same way. All information as well as advanced topics can be found directly on Google in the [corresponding documentation](https://developer.android.com/topic/libraries/architecture/room.html).