---
title: 'Database Modeling with dbdiagram.io'
date: 2019-03-31T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2019/03/database-modeling-with-dbdiagram-io
categories:
  - tools
tags:
  - tools
  - apps
  - database
  - modeling
---

Today I would like to briefly introduce you to a Web application, which I have been stumpled across. 
I've tried several tools for db modeling, some more and some less appealing.

Well, the tool doesn't offer explicit ER modeling, 
but a SQL-like structure and allows you to import and export directly to SQL and other formats.

You can find the app I'm talking about on [dbdiagram.io](dbdiagram.io). 
The website offers a simple graphical interface, which includes a code editor and a graphical representation of the table structure. 
You can edit your table structure directly in the code editor. 
You can then create relations between the tables using the drag-and-drop function. 
These are represented in the editor by the schema *ref "table_a". "column_a" > "table_b". "column_b"*. 
Cardinalities are represented by > (n:1) , < (1:n) and - (1:1).

![dbdiagram.io GUI](/assets/2019/dbdiagram_gui.png)

The syntax is quite simple, just follow the schema

```sql
Table tablename as t {
  columnname datatype [constraints]
  .....
}
```

```sql
Table tablename as t {
  columnname datatype [constraints]
  .....
}
```

You can use the constraints *primary key*, *not null* and *unique*. 
Multiple constraints can be used comma-separated. 
You can add aliases for tablenames by using *as aliasname*, but this is entirely optional. 
And that's basically it! For more information, you can check out the projects [introductory article](https://hackernoon.com/dbdiagram-io-a-database-diagram-designer-built-for-developers-and-analysts-975f310d4f13) or just try it out yourself - it's free!

All in all, the tool is very self-explanatory and clear, 
but in my opinion offers a simple way to provide a quick overview of your database and can be easily used for your own projects 
due to the import and export functionality.
