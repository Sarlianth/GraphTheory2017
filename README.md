# GraphTheory2017
Under-graduate project using neo4j for "Graph Theory" module

<h2>Project requirements</h2>
<pre>
The minimum standard for this project is a GitHub repository containing
a document detailing the design of the database and a prototype Neo4j
database following that design and using data from GMIT.
The document should clearly state what data needs to be stored by a
timetabling system, and how that data is to be represented in the database.
This means that you must clearly specify the design choices you have made
as to what data are represented as nodes, relationships, labels, relationship
types or properties. It should also specify, and give examples of, Cypher
queries for creating, retrieving, updating and deleting data.
The prototype database must contain data from GMIT’s own timetabling
system, in the structure set out in your design document. The data can be
copied or scraped from GMIT’s timetabling website, or otherwise obtained.
You should include, as a separate section in your design document, how you
obtained the data in your prototype database.
A better project will be well organised and contain detailed explanations.
The architecture of the system will be well conceived, and example queries
for various tasks, such as optimising timetables, will be provided.
</pre>

<h2>What is Neo4J</h2>
Sponsored by Neo Technology, Neo4j is an open-source NoSQL graph database implemented in Java and Scala. With development starting in 2003, it has been publicly available since 2007. The source code and issue tracking are available on GitHub, with support readily available on Stack Overflow and the Neo4j Google group. Neo4j is used today by hundreds of thousands of companies and organizations in almost all industries. Use cases include matchmaking, network management, software analytics, scientific research, routing, organizational and project management, recommendations, social networks, and more.

![graphmodel](https://cloud.githubusercontent.com/assets/10263556/25135725/0c814e94-244b-11e7-939f-6ebee1122a08.jpg)

<h2>Database Design</h2>
I have been thinking and planning how I should design the database so it's very easy to understand and refer to. In the diagrams that I have created below I would like to show you how I decided to design it.

![graph](https://cloud.githubusercontent.com/assets/10263556/25136943/43928e2c-244e-11e7-9e9c-e9a8a537e133.jpg)

I am going to create all nodes seperately, and afterwards I will make relationships between them.

The main node is going to be `MODULE`, as everything will be related to this particular node. 
The structure of the model will look as follows: `Course` -has> `Semester` -has> `Module` (-thought_by-> Lecturer) -has> `Room` -have> `Day` -have> `Time`

Lecturers are related to the modules they teach
![lecturers-modules](https://cloud.githubusercontent.com/assets/10263556/25140864/95edcc1c-2459-11e7-94c4-d265f9dd8481.jpg)

Course has different semesters (in our case, Software Development course contains 8 different semesters [4 years])
![course-semesters](https://cloud.githubusercontent.com/assets/10263556/25140900/ba4865a4-2459-11e7-8e48-a85a78c05ce5.jpg)

Each of the 8 semesters contains different modules
![semester-module](https://cloud.githubusercontent.com/assets/10263556/25140921/cccf7e7e-2459-11e7-9a7e-39b552d9d57b.jpg)

<h2>Data</h2>
In order to obtain the data, I have used the official GMIT website: http://timetable.gmit.ie and I have just selected the years and semesters subsequently copying the data from the website. To obtain the data faster I have inspected the element on the website and then copied the outerHTML and pasting it into a text document.

![timetable](https://cloud.githubusercontent.com/assets/10263556/25144820/06ecb164-2467-11e7-9c52-2e3991e1bdeb.jpg)

All the data that I have used for this project has been obtained from the timetable above.

<h2>Commit 10</h2>
In commit number 10 I have decided to relate all the modules with corresponding rooms, so that I could see some progress when I query the database. The graph below represents what the database looks like at the moment (I did not relate days monday-friday and corresponding times to the rooms yet).

![database](https://cloud.githubusercontent.com/assets/10263556/25142330/6af0706e-245e-11e7-821e-3d4ba18be928.jpg)

<h2>Finished graph</h2>
The finished graph with relationships that I have designed looks as follows:

![graphtotal](https://cloud.githubusercontent.com/assets/10263556/25143994/50a75d16-2464-11e7-9508-ed8127fdb217.jpg)

After I have created the graph and queried to display all, it does look kind of messy to me. If I was to approach this project again I think I would try to avoid representing time as relationship. However, I think the way I have designed it is clear enough to understand the graph and read the timetable off it.

<h2>Cypher commands</h2>
To return all nodes and see the complete graph
```cypher
MATCH (n) RETURN n
```

To delete all the nodes also with relationships 
```cypher
MATCH (n) DETACH DELETE n
```

To delete a node with X ID 
```cypher
MATCH (n) WHERE id(n) = X DETACH DELETE n
```

To create relationships between nodes
```cypher
MATCH (u:User {username:'admin'}), (r:Role {name:'ROLE_WEB_USER'}) CREATE (u)-[:HAS_ROLE]->(r)
```

To update existing node
```cypher
merge (n:Node {name: 'John'})
set n += {age: 34, coat: 'Yellow'}
return n 
```

Creating a department called Science:
```cypher
create (a:Department { name: "Science"})
```

Create a course called Software Development
```cypher
create (a:Course { name:"Software Development" })
```

Creating a relationship between the two nodes
```cypher
match (a:Department),(b:Course) where a.name = "Science" and b.name = "Software Development" create (a)-[r:Contains]->(b)
```

Create a module taught by XX lecturer
```cypher
create (a:Module { name: "Graph Theory", Lecturer: "Ian McLoughlin" })
```

Create a relationship between the course and a module
```cypher
match (a:Course),(b:Module) where a.name = "Software Development" and b.name = "Graph Theory" create (a)-[r:Contains]->(b)
```
Create all the different modules and add relationships between modules and course
```cypher
match (a:Course) where a.name = "Software Development" 
create (b:Module { name: "Database Management Systems", Lecturer: "Deirdre O'Donovan"}), 
(c:Module { name: "Mobile Applications Development 2", Lecturer: "Damian Costello"}), 
(d:Module { name: "Software Testing", Lecturer: "Martin Hynes"}), 
(e:Module { name: "Server Side Rad", Lecturer: "Gerard Harrison"}), 
(f:Module { name: "Professional Practice In IT", Lecturer: "Damian Costello"}), 
(a)-[:Contains]->(b), (a)-[:Contains]->(c), (a)-[:Contains]->(d), (a)-[:Contains]->(e), (a)-[:Contains]->(f)
```
