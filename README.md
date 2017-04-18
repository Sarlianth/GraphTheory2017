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
