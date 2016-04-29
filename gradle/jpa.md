#Java Persistence Api

##Introduction

### Object/Relational Mapping
- A task to map a Java object with their attributes with relational database table and columns for all database task using object approach.

### Mismatches Between Relational and Object Models

Relational objects are represented in a tabular format, while object models are represented in an interconnected graph of object format. While storing and retrieving an object model from a relational database, some mismatch occurs due to the following reasons:

- Granularity : Sometimes Object model has more class needed than database table . Object model have more granularity than relational model.

- Subtypes : Inheritance paradigm are not supported by all types of relational databases. RDBMS usually normalize inheritance using additional type column, or additional related table with additional child specific columns.

- Identity : Like object model, relational model does not expose identity while writing equality. A RDBMS defines exactly one notion of 'sameness': the primary key. Java, however, defines both object identity a==b and object equality a.equals(b).

- Associations : Relational models cannot determine multiple relationships while looking into an object domain model.

- Data navigation : Data navigation between objects in an object network is different in both models.

### The Java Persistence API
Provide by Oracle, Java persistenece API is a collection of classes and methods to manage large amount of data persistenly in database.  

### JPA History
- Java Community Process(JSR) release Enterprise JavaBeans 3.0 
- JPA 1.0 as part of work of EJB 3.0 JSR 220 Expert group
- JPA 2.0 as part of work of JSR 317 Expert group
- JPA 2.1 as part of work of JSR 338 Expert group

### JPA Architecture
- Core classes and exceptions in package javax.persistence  

![JPA Core Architecture](images/jpa-arch.png)
![JPA Exception Architecture](images/jpa-exceptions.png)

- EntityManagerFactory  
  This is a factory class of EntityManager. It creates and manages multiple EntityManager instances.
- EntityManager  
  It is an Interface, it manages the persistence operations on objects. It works like factory for Query instance.
- Entity  
  Entities are the persistence objects, stores as records in the database.
- EntityTransaction  
  It has one-to-one relationship with EntityManager. For each EntityManager, operations are maintained by EntityTransaction class.
- Persistence  
  This class contain static methods to obtain EntityManagerFactory instance.
- Query  
  This interface is implemented by each JPA vendor to obtain relational objects that meet the criteria.
  
### Entity Metadata

#### Entities

* An entity is a lightweight persistence domain object.
* Typically an entity represents a table in a relational database, and each entity instance corresponds to a row in that table.
* The primary programming artifact of an entity is the entity class, although entities can use helper classes.

#### Requirements for Entity Classes

- The class must be annotated with the javax.persistence.Entity annotation.

- The class must have a public or protected, no-argument constructor. The class may have other constructors.

- The class must not be declared final. No methods or persistent instance variables must be declared final.

- If an entity instance be passed by value as a detached object, such as through a session bean’s remote business interface, the class must implement the Serializable interface.

- Entities may extend both entity and non-entity classes, and non-entity classes may extend entity classes.

- Persistent instance variables must be declared private, protected, or package-private, and can only be accessed directly by the entity class’s methods. Clients must access the entity’s state through accessor or business methods.

#### Persistent Fields and Properties in Entity Classes

- The persistent state of an entity can be accessed either through the entity’s instance variables or through JavaBeans-style properties. The fields or properties must be of the following Java language types:

- Java primitive types

- java.lang.String

- Other serializable types including:

	Wrappers of Java primitive types

	java.math.BigInteger

	java.math.BigDecimal

	java.util.Date

	java.util.Calendar

	java.sql.Date

	java.sql.Time

	java.sql.TimeStamp

	User-defined serializable types

	byte[]

	Byte[]

	char[]

	Character[]

- Enumerated types

- Other entities and/or collections of entities

- Embeddable classes


#### Persistent Fields
- If the entity class uses persistent fields, the Persistence runtime accesses entity class instance variables directly. All fields not annotated javax.persistence.Transient or not marked as Java transient will be persisted to the data store. The object/relational mapping annotations must be applied to the instance variables.

#### Persistent Properties

- If the entity uses persistent properties, the entity must follow the method conventions of JavaBeans components. JavaBeans-style properties use getter and setter methods that are typically named after the entity class’s instance variable names. For every persistent property property of type Type of the entity, there is a getter method getProperty and setter method setProperty. If the property is a boolean, you may use isProperty instead of getProperty. For example, if a Customer entity uses persistent properties, and has a private instance variable called firstName, the class defines a getFirstName and setFirstName method for retrieving and setting the state of the firstName instance variable.

- The method signature for single-valued persistent properties are as follows:

  ```java
Type getProperty()
void setProperty(Type type)
```

- The object/relational mapping annotations for must be applied to the getter methods. Mapping annotations cannot be applied to fields or properties annotated @Transient or marked transient.

#### Using Collections in Entity Fields and Properties
- Collection-valued persistent fields and properties must use the supported Java collection interfaces regardless of whether the entity uses persistent fields or properties. The following collection interfaces may be used:

	java.util.Collection

	java.util.Set

	java.util.List

	java.util.Map

- If the entity class uses persistent fields, the type in the preceding method signatures must be one of these collection types. Generic variants of these collection types may also be used. For example, if it has a persistent property that contains a set of phone numbers, the Customer entity would have the following methods:  

  ```java
Set<PhoneNumber> getPhoneNumbers() { ... }
void setPhoneNumbers(Set<PhoneNumber>) { ... }
```
- If a field or property of an entity consists of a collection of basic types or embeddable classes, use the javax.persistence.ElementCollection annotation on the field or property.

- The two attributes of @ElementCollection are targetClass and fetch. The targetClass attribute specifies the class name of the basic or embeddable class and is optional if the field or property is defined using Java programming language generics. The optional fetch attribute is used to specify whether the collection should be retrieved lazily or eagerly, using the javax.persistence.FetchType constants of either LAZY or EAGER, respectively. By default, the collection will be fetched lazily.

- The following entity, Person, has a persistent field, nicknames, which is a collection of String classes that will be fetched eagerly. The targetClass element is not required, because it uses generics to define the field.  

  ```java
@Entity
public class Person {
    ...
    @ElementCollection(fetch=EAGER)
    protected Set<String> nickname = new HashSet();
    ...
}
```

### The Entity Manager
Entity manager is JPA interface to interact with the persistence context. An instance of EntityManager used to create, remove, find, or query entities. To get an EntityManager we need to create from EntityManager Factory. While EntityManagerFactory created by calling  "Persistence.createEntityManagerFactory( "persistence_unit_name" );". The persistence unit name defined in persistence.xml file.
List of method in EntityManager interface: http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html


### JPA Providers
JPA API is a interface specification while to make it work need JPA provider for implementation.

JPA Implementation:
- Hibernate (most widely used and Mitrais Recommendation)
- TopLink
- EclipseLink
- Apache OpenJPA
- ObjectDB
- DataNucleaus

## Object/Relational Mapping

### Annotations
Normally we use xml to configure the JPA mapping of POJO with JPA configuration, this is a separated xml file from the Java files. With annotation we can write configuration part directly on the class definition. Annotations start with '@' symbol. They are declared before class, field/property declaration. All JPA annotation defined in javax.persistence package  

Here is the list of most used JPA Annotation  

<table>
   <tbody>
      <tr>
         <th>Annotation</th>
         <th>Description</th>
      </tr>
      <tr>
         <td>@Entity</td>
         <td>This annotation specifies to declare the class as entity or a table.</td>
      </tr>
      <tr>
         <td>@Table</td>
         <td>This annotation specifies to declare table name.</td>
      </tr>
      <tr>
         <td>@Basic</td>
         <td>This annotation specifies non constraint fields explicitly.</td>
      </tr>
      <tr>
         <td>@Embedded</td>
         <td>This annotation specifies the properties of class or an entity whose value instance of an embeddable class.</td>
      </tr>
      <tr>
         <td>@Id</td>
         <td>This annotation specifies the property, use for identity (primary key of a table) of the class.</td>
      </tr>
      <tr>
         <td>@GeneratedValue</td>
         <td>This annotation specifies, how the identity attribute can be initialized such as Automatic, manual, or value taken from sequence table.</td>
      </tr>
      <tr>
         <td>@Transient</td>
         <td>This annotation specifies the property which in not persistent i.e. the value is never stored into database.</td>
      </tr>
      <tr>
         <td>@Column</td>
         <td>This annotation is used to specify column or attribute for persistence property.</td>
      </tr>
      <tr>
         <td>@SequenceGenerator</td>
         <td>This annotation is used to define the value for the property which is specified in @GeneratedValue annotation. It creates a sequence.</td>
      </tr>
      <tr>
         <td>@TableGenerator</td>
         <td>This annotation is used to specify the value generator for property specified in @GeneratedValue annotation. It creates a table for value generation.</td>
      </tr>
      <tr>
         <td>@AccessType</td>
         <td>This type of annotation is used to set the access type. If you set @AccessType(FIELD) then Field wise access will occur. If you set @AccessType(PROPERTY) then Property wise assess will occur.</td>
      </tr>
      <tr>
         <td>@JoinColumn</td>
         <td>This annotation is used to specify an entity association or entity collection. This is used in many- to-one and one-to-many associations.</td>
      </tr>
      <tr>
         <td>@UniqueConstraint</td>
         <td>This annotation is used to specify the field, unique constraint for primary or secondary table.</td>
      </tr>
      <tr>
         <td>@ColumnResult</td>
         <td>This annotation references the name of a column in the SQL query using select clause.</td>
      </tr>
      <tr>
         <td>@ManyToMany</td>
         <td>This annotation is used to define a many-to-many relationship between the join Tables.</td>
      </tr>
      <tr>
         <td>@ManyToOne</td>
         <td>This annotation is used to define a many-to-one relationship between the join Tables.</td>
      </tr>
      <tr>
         <td>@OneToMany</td>
         <td>This annotation is used to define a one-to-many relationship between the join Tables.</td>
      </tr>
      <tr>
         <td>@OneToOne</td>
         <td>This annotation is used to define a one-to-one relationship between the join Tables.</td>
      </tr>
      <tr>
         <td>@NamedQueries</td>
         <td>This annotation is used for specifying list of named queries.</td>
      </tr>
      <tr>
         <td>@NamedQuery</td>
         <td>This annotation is used for specifying a Query using static name.</td>
      </tr>
   </tbody>
</table>

  
### JavaBean Standards

JavaBeans are classes that encapsulate many objects into a single object (the bean). They are serializable, have a zero-argument constructor, and allow access to properties using getter and setter methods. The name "Bean" was given to encompass this standard, which aims to create reusable software components for Java.

JavaBean Convention
- Bean contains default constructor without arguments  
- To access properties need using get(accessor), set(mutator), is(for boolean accessor).  
- Accessor or Mutator method name start with small lettered accessor or mutator following with the field name started with capital first letter.
  Example:  

  ```java
public void setName(String argName){...}  
public void String getName(){...}  
public boolean isHeadFamily(){...}  
public void setHeadFamily(boolean argHeadFamily){...}
```

### Property, Field, and Mixed Access

- The persistence implementation must be able to retrieve and set the persistent state of your entities, mapped superclasses, and embeddable types. JPA offers two modes of persistent state access: field access, and property access. Under field access, the implementation injects state directly into your persistent fields, and retrieves changed state from your fields as well. To declare field access on an entity with XML metadata, set the access attribute of your entity XML element to FIELD. To use field access for an entity using annotation metadata, simply place your metadata and mapping annotations on your field declarations:

   ```java
@ManyToOne
private Company publisher;
```
- Property access, on the other hand, retrieves and loads state through JavaBean "getter" and "setter" methods. For a property p of type T, you must define the following getter method:

   ```java
T getP();
```
For boolean properties, this is also acceptable:
   ```java
boolean isP();
   ```
You must also define the following setter method:
   ```java
void setP(T value);
```
- To use property access, set your entity element's access attribute to PROPERTY, or place your metadata and mapping annotations on the getter method:

   ```java
@ManyToOne
private Company getPublisher() { ... }


private void setPublisher(Company publisher) { ... }
```

Warning

- When using property access, only the getter and setter method for a property should ever access the underlying persistent field directly. Other methods, including internal business methods in the persistent class, should go through the getter and setter methods when manipulating persistent state.

- Also, take care when adding business logic to your getter and setter methods. Consider that they are invoked by the persistence implementation to load and retrieve all persistent state; other side effects might not be desirable.

- Each class must use either field access or property access for all state; you cannot use both access types within the same class. Additionally, a subclass must use the same access type as its superclass.

- Mixed Access Mode

Though you will not require to mix modes in most of the scenarios, but it is possible and useful in some cases. For example, when an entity subclass is added to an existing hierarchy that uses a different access type. Adding an @Access annotation with a specified access mode on the subclass entity (or even field) will cause the default access type to be overridden for that entity subclass.

### Table and Column Mapping
### Primary Keys and Generation
- Primary key should be one of the Java primitive type
- Date primary key, temporal type should be spesified as DATE.
- Composite Primary Key Rules:
  * The primary key class must be public and must have a public no-arg constructor.
  * The access type (field- or property-based access) of a primary key class is determined by the access type of the entity for which it is the primary key unless the primary key is a embedded id and a different access type is specified. 
  * If property-based access is used, the properties of the primary key class must be public or protected.
  * The primary key class must be serializable.
  * The primary key class must define equals and hashCode methods. The semantics of value equality for these methods must be consistent with the database equality for the database types to which the key is mapped.
  * A composite primary key must either be represented and mapped as an embeddable class  or must be represented as an id class and mapped to multiple fields or properties of the entity class .
  * If the composite primary key class is represented as an id class, the names of primary key fields or properties in the primary key class and those of the entity class to which the id class is mapped must correspond and their types must be the same.
  * A primary key that corresponds to a derived identity must conform to the rules of.
  
### Type Mappings
- Basic DataType to Java Type Mapping.  
   MS SQL Server: https://msdn.microsoft.com/en-us/library/ms378878(v=sql.110).aspx  
   
  <table responsive="true">
   <thead>
      <tr responsive="true">
         <th>SQL Server Types</th>
         <th>JDBC Types (java.sql.Types)</th>
         <th>Java Language Types</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td data-th="SQL Server Types">bigint</td>
         <td data-th="JDBC Types (java.sql.Types)">BIGINT</td>
         <td data-th="Java Language Types">long</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">binary</td>
         <td data-th="JDBC Types (java.sql.Types)">BINARY</td>
         <td data-th="Java Language Types">byte[]</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">bit</td>
         <td data-th="JDBC Types (java.sql.Types)">BIT</td>
         <td data-th="Java Language Types">boolean</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">char</td>
         <td data-th="JDBC Types (java.sql.Types)">CHAR</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">date</td>
         <td data-th="JDBC Types (java.sql.Types)">DATE</td>
         <td data-th="Java Language Types">java.sql.Date</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">datetime</td>
         <td data-th="JDBC Types (java.sql.Types)">TIMESTAMP</td>
         <td data-th="Java Language Types">java.sql.Timestamp</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">datetime2</td>
         <td data-th="JDBC Types (java.sql.Types)">TIMESTAMP</td>
         <td data-th="Java Language Types">java.sql.Timestamp</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">datetimeoffset (2)</td>
         <td data-th="JDBC Types (java.sql.Types)">microsoft.sql.Types.DATETIMEOFFSET</td>
         <td data-th="Java Language Types">microsoft.sql.DateTimeOffset</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">decimal</td>
         <td data-th="JDBC Types (java.sql.Types)">DECIMAL</td>
         <td data-th="Java Language Types">java.math.BigDecimal</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">float</td>
         <td data-th="JDBC Types (java.sql.Types)">DOUBLE</td>
         <td data-th="Java Language Types">double</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">image</td>
         <td data-th="JDBC Types (java.sql.Types)">LONGVARBINARY</td>
         <td data-th="Java Language Types">byte[]</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">int</td>
         <td data-th="JDBC Types (java.sql.Types)">INTEGER</td>
         <td data-th="Java Language Types">int</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">money</td>
         <td data-th="JDBC Types (java.sql.Types)">DECIMAL</td>
         <td data-th="Java Language Types">java.math.BigDecimal</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">nchar</td>
         <td data-th="JDBC Types (java.sql.Types)">CHAR<br><br> NCHAR (Java SE 6.0)</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">ntext</td>
         <td data-th="JDBC Types (java.sql.Types)">LONGVARCHAR<br><br> LONGNVARCHAR (Java SE 6.0)</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">numeric</td>
         <td data-th="JDBC Types (java.sql.Types)">NUMERIC</td>
         <td data-th="Java Language Types">java.math.BigDecimal</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">nvarchar</td>
         <td data-th="JDBC Types (java.sql.Types)">VARCHAR<br><br> NVARCHAR (Java SE 6.0)</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">nvarchar(max)</td>
         <td data-th="JDBC Types (java.sql.Types)">VARCHAR<br><br> NVARCHAR (Java SE 6.0)</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">real</td>
         <td data-th="JDBC Types (java.sql.Types)">REAL</td>
         <td data-th="Java Language Types">float</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">smalldatetime</td>
         <td data-th="JDBC Types (java.sql.Types)">TIMESTAMP</td>
         <td data-th="Java Language Types">java.sql.Timestamp</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">smallint</td>
         <td data-th="JDBC Types (java.sql.Types)">SMALLINT</td>
         <td data-th="Java Language Types">short</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">smallmoney</td>
         <td data-th="JDBC Types (java.sql.Types)">DECIMAL</td>
         <td data-th="Java Language Types">java.math.BigDecimal</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">text</td>
         <td data-th="JDBC Types (java.sql.Types)">LONGVARCHAR</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">time</td>
         <td data-th="JDBC Types (java.sql.Types)">TIME (1)</td>
         <td data-th="Java Language Types">java.sql.Time (1)</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">timestamp</td>
         <td data-th="JDBC Types (java.sql.Types)">BINARY</td>
         <td data-th="Java Language Types">byte[]</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">tinyint</td>
         <td data-th="JDBC Types (java.sql.Types)">TINYINT</td>
         <td data-th="Java Language Types">short</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">udt</td>
         <td data-th="JDBC Types (java.sql.Types)">VARBINARY</td>
         <td data-th="Java Language Types">byte[]</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">uniqueidentifier</td>
         <td data-th="JDBC Types (java.sql.Types)">CHAR</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">varbinary</td>
         <td data-th="JDBC Types (java.sql.Types)">VARBINARY</td>
         <td data-th="Java Language Types">byte[]</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">varbinary(max)</td>
         <td data-th="JDBC Types (java.sql.Types)">VARBINARY</td>
         <td data-th="Java Language Types">byte[]</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types"></td>
         <td data-th="JDBC Types (java.sql.Types)"></td>
         <td data-th="Java Language Types"></td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">varchar</td>
         <td data-th="JDBC Types (java.sql.Types)">VARCHAR</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">varchar(max)</td>
         <td data-th="JDBC Types (java.sql.Types)">VARCHAR</td>
         <td data-th="Java Language Types">String</td>
      </tr>
      <tr>
         <td data-th="SQL Server Types">xml</td>
         <td data-th="JDBC Types (java.sql.Types)">LONGVARCHAR<br><br> LONGNVARCHAR (Java SE 6.0)</td>
         <td data-th="Java Language Types">String<br><br> SQLXML</td>
      </tr>
   </tbody>
</table>

   ORACLE, PL/QL : https://docs.oracle.com/cd/B19306_01/java.102/b14188/datamap.htm  
  <table title="SQL and PL/SQL Data Type to Oracle and JDBC Mapping Classes " summary="This table lists the Oracle mapping type and the JDBC mapping type for each SQL and PL/SQL datatype.">
   <thead>
      <tr>
         <th>SQL and PL/SQL Data Type</th>
         <th>Oracle Mapping</th>
         <th>JDBC Mapping</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            <p><code>CHAR</code>, <code>CHARACTER</code>, <code>LONG</code>, <code>STRING</code>, <code>VARCHAR</code>, <code>VARCHAR2</code></p>
         </td>
         <td>
            <p><code>oracle.sql.CHAR</code></p>
         </td>
         <td>
            <p><code>java.lang.String</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>NCHAR</code>, <code>NVARCHAR2</code></p>
         </td>
         <td>
            <p><code>oracle.sql.NCHAR</code> (note 1)</p>
         </td>
         <td>
            <p><code>oracle.sql.NString</code> (note 1)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>NCLOB</code></p>
         </td>
         <td>
            <p><code>oracle.sql.NCLOB</code> (note 1)</p>
         </td>
         <td>
            <p><code>oracle.sql.NCLOB</code> (note 1)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>RAW</code>, <code>LONG RAW</code></p>
         </td>
         <td>
            <p><code>oracle.sql.RAW</code></p>
         </td>
         <td>
            <p><code>byte[]</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>BINARY_INTEGER</code>, <code>NATURAL</code>, <code>NATURALN</code>, <code>PLS_INTEGER</code>, <code>POSITIVE</code>, <code>POSITIVEN</code>, <code>SIGNTYPE</code>, <code>INT</code>, <code>INTEGER</code></p>
         </td>
         <td>
            <p><code>oracle.sql.NUMBER</code></p>
         </td>
         <td>
            <p><code>int</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>DEC</code>, <code>DECIMAL</code>, <code>NUMBER</code>, <code>NUMERIC</code></p>
         </td>
         <td>
            <p><code>oracle.sql.NUMBER</code></p>
         </td>
         <td>
            <p><code>java.math.BigDecimal</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>DOUBLE PRECISION</code>, <code>FLOAT</code></p>
         </td>
         <td>
            <p><code>oracle.sql.NUMBER</code></p>
         </td>
         <td>
            <p><code>double</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>SMALLINT</code></p>
         </td>
         <td>
            <p><code>oracle.sql.NUMBER</code></p>
         </td>
         <td>
            <p><code>int</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>REAL</code></p>
         </td>
         <td>
            <p><code>oracle.sql.NUMBER</code></p>
         </td>
         <td>
            <p><code>float</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>DATE</code></p>
         </td>
         <td>
            <p><code>oracle.sql.DATE</code></p>
         </td>
         <td>
            <p><code>java.sql.Timestamp</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>TIMESTAMP</code></p>
            <p><code>TIMESTAMP WITH TZ</code></p>
            <p><code>TIMESTAMP WITH LOCAL TZ</code></p>
         </td>
         <td>
            <p><code>oracle.sql.TIMESTAMP</code></p>
            <p><code>oracle.sql.TIMESTAMPTZ</code></p>
            <p><code>oracle.sql.TIMESTAMPLTZ</code></p>
         </td>
         <td>
            <p><code>java.sql.Timestamp</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>INTERVAL YEAR TO MONTH</code></p>
            <p><code>INTERVAL DAY TO SECOND</code></p>
         </td>
         <td>
            <p><code>String</code> (note 2)</p>
         </td>
         <td>
            <p><code>String</code> (note 2)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>ROWID</code>, <code>UROWID</code></p>
         </td>
         <td>
            <p><code>oracle.sql.ROWID</code></p>
         </td>
         <td>
            <p><code>oracle.sql.ROWID</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>BOOLEAN</code></p>
         </td>
         <td>
            <p><code>boolean</code> (note 3)</p>
         </td>
         <td>
            <p><code>boolean</code> (note 3)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>CLOB</code></p>
         </td>
         <td>
            <p><code>oracle.sql.CLOB</code></p>
         </td>
         <td>
            <p><code>java.sql.Clob</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>BLOB</code></p>
         </td>
         <td>
            <p><code>oracle.sql.BLOB</code></p>
         </td>
         <td>
            <p><code>java.sql.Blob</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>BFILE</code></p>
         </td>
         <td>
            <p><code>oracle.sql.BFILE</code></p>
         </td>
         <td>
            <p><code>oracle.sql.BFILE</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p>Object types</p>
         </td>
         <td>
            <p>Generated class</p>
         </td>
         <td>
            <p>Generated class</p>
         </td>
      </tr>
      <tr>
         <td>
            <p>SQLJ object types</p>
         </td>
         <td>
            <p>Java class defined at type creation</p>
         </td>
         <td>
            <p>Java class defined at type creation</p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>OPAQUE</code> types</p>
         </td>
         <td>
            <p>Generated or predefined class (note 4)</p>
         </td>
         <td>
            <p>Generated or predefined class (note&nbsp;4)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>RECORD</code> types</p>
         </td>
         <td>
            <p>Through mapping to SQL object type (note 5)</p>
         </td>
         <td>
            <p>Through mapping to SQL object type (note 5)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p>Nested table, <code>VARRAY</code></p>
         </td>
         <td>
            <p>Generated class implemented using <code>oracle.sql.ARRAY</code></p>
         </td>
         <td>
            <p><code>java.sql.Array</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p>Reference to object type</p>
         </td>
         <td>
            <p>Generated class implemented using <code>oracle.sql.REF</code></p>
         </td>
         <td>
            <p><code>java.sql.Ref</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p><code>REF CURSOR</code></p>
         </td>
         <td>
            <p><code>java.sql.ResultSet</code></p>
         </td>
         <td>
            <p><code>java.sql.ResultSet</code></p>
         </td>
      </tr>
      <tr>
         <td>
            <p>Index-by tables</p>
         </td>
         <td>
            <p>Through mapping to SQL collection (note 6)</p>
         </td>
         <td>
            <p>Through mapping to SQL collection (note 6)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p>Scalar (numeric or character)</p>
            <p>Index-by tables</p>
         </td>
         <td>
            <p>Through mapping to Java array (note 7)</p>
         </td>
         <td>
            <p>Through mapping to Java array (note&nbsp;7)</p>
         </td>
      </tr>
      <tr>
         <td>
            <p>User-defined subtypes</p>
         </td>
         <td>
            <p>Same as for base type</p>
         </td>
         <td>
            <p>Same as for base type</p>
         </td>
      </tr>
   </tbody>
</table>


   
### Temporal and Enumerated Types
####Temporal Type  
When dealing with persistent field with java.util.Date or java.util.Calendar we need to convert to database compatible column type to store and retrieve from database. Temporal type have 3 enum value.  
- TemporalType.DATE (equal to java.sql.Date)  

  ```java
@Temporal(TemporalType.DATE)
@Column(name = "START_DATE")
private java.util.Date startDate;
```
- TemporalType.TIME (equal to java.sql.Time)  

  ```java
@Temporal(TemporalType.TIME)
@Column(name = "START_DATE")
private java.util.Date startDate;
```
- TemporalType.TIMESTAMP (equal to java.sql.TimeStamp)  

  ```java
@Temporal(TemporalType.TIMESTAMP)
@Column(name = "START_DATE")
private java.util.Date startDate;
```

#### Enumerated Types
An Enumerated annotation specifies that a persistent property or field should be persisted as a enumerated
type. The Enumerated annotation may be used in conjunction with the Basic annotation.
The Enumerated annotation may be used in conjunction with the ElementCollection
annotation when the element collection value is of basic type.
An enum can be mapped as either a string or an integer. The EnumType enum defines the mapping
for enumerated types.  

```java
public enum EnumType {
ORDINAL,
STRING
}
```

If the enumerated type is not specified or the Enumerated annotation is not used, the enumerated type
is assumed to be ORDINAL unless a converter is being applied. 
 
```java
@Target({METHOD, FIELD}) @Retention(RUNTIME)
public @interface Enumerated {
EnumType value() default ORDINAL;
}
```

```java
public enum EmployeeStatus {FULL_TIME, PART_TIME, CONTRACT}
public enum SalaryRate {JUNIOR, SENIOR, MANAGER, EXECUTIVE}
@Entity public class Employee {
...
public EmployeeStatus getStatus() {...}
@Enumerated(STRING)
public SalaryRate getPayScale() {...}
...
}
```

If the status property is mapped to a column of integer type, and the payscale property to a column of
varchar type, an instance will have atribute value as below.  
FULL_TIME	| status = 0  
PART_TIME	| status = 1  
CONTRACT	| status = 2  

JUNIOR		| payScale = "JUNIOR"  
SENIOR		| payScale = "SENIOR"  
MANAGER		| payScale = "MANAGER"  
EXECUTIVE	| payScale = "EXECUTIVE"  

Notes
- Ordinal enum need to be ordered and new value must added at last position, enum order changes will cause value changes and not valid overtime
- Avoid ordinal enum if order value is significant to your code
 
### Embedded Types
Embedded Types is a class that have attribute that can be used as part attribute with other persistence class. In this sample we use Periode class as Embeddable class.

```java
@Embeddable
public class Period {
	private Date startDate;
	private Date endDate;

	@Column(name ="START_DATE")
	public Date getStartDate() {
		return startDate;
	}

	public void setStartDate(Date startDate) {
		this.startDate = startDate;
	}

	@Column(name ="END_DATE")
	public Date getEndDate() {
		return endDate;
	}

	public void setEndDate(Date endDate) {
		this.endDate = endDate;
	}
}
```
We used Period as part of Project persistence class.

```java
@Entity
@Table(name = "PROJECT")
public class Project {
	private Long id;
	private String title;
	private Period projectPeriod;
	
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}
	
	@Column(name = "TITLE")
	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}
	
	@Embedded
	public Period getProjectPeriod() {
		return projectPeriod;
	}

	public void setProjectPeriod(Period projectPeriod) {
		this.projectPeriod = projectPeriod;
	}
```

In database PROJECT table have columns(ID,TITLE,START_DATE,END_DATE) while there is no PERIOD table. We can embed Period class to other Entity class that have same START_DATE and END_DATE column in it's related table. So we don't need to rewrite those two identical additional attribute again.

### Entity Relationships



### @ManyToOne Relationships
This annotation defines a single-valued association to another entity class that has many-to-one multiplicity. It is not normally necessary to specify the target entity explicitly since it can usually be inferred from the type of the object being referenced.

```java
     Example:

     @ManyToOne(optional=false) 
     @JoinColumn(name="CUST_ID", nullable=false, updatable=false)
     public Customer getCustomer() { return customer; }
```

### @OneToOne Relationships

This annotation defines a single-valued association to another entity that has one-to-one multiplicity. It is not normally necessary to specify the associated target entity explicitly since it can usually be inferred from the type of the object being referenced.

Example 1: One-to-one association that maps a foreign key column

On Customer class:

```java
    @OneToOne(optional=false)
    @JoinColumn(
        name="CUSTREC_ID", unique=true, nullable=false, updatable=false)
    public CustomerRecord getCustomerRecord() { return customerRecord; }
```
On CustomerRecord class:

```java
    @OneToOne(optional=false, mappedBy="customerRecord")
    public Customer getCustomer() { return customer; }
```
    Example 2: One-to-one association that assumes both the source and target share the same primary key values. 

On Employee class:

```java
    @Entity
    public class Employee {
        @Id Integer id;
    
        @OneToOne @PrimaryKeyJoinColumn
        EmployeeInfo info;
        ...
    }
```
    On EmployeeInfo class:
```java
    @Entity
    public class EmployeeInfo {
        @Id Integer id;
        ...
    }
```
### @OneToMany Relationships
Defines a many-valued association with one-to-many multiplicity.

If the collection is defined using generics to specify the element type, the associated target entity type need not be specified; otherwise the target entity class must be specified.


Example 1: One-to-Many association using generics  
In Customer class:

```java
    @OneToMany(cascade=ALL, mappedBy="customer")
    public Set getOrders() { return orders; }
```
In Order class:

```java
    @ManyToOne
    @JoinColumn(name="CUST_ID", nullable=false)
    public Customer getCustomer() { return customer; }
```
Example 2: One-to-Many association without using generics  
In Customer class:

```java
    @OneToMany(targetEntity=com.acme.Order.class, cascade=ALL,
            mappedBy="customer")
    public Set getOrders() { return orders; }
```
In Order class:

```java
    @ManyToOne
    @JoinColumn(name="CUST_ID", nullable=false)
    public Customer getCustomer() { return customer; }
```
### @ManyToMany Relationships

In Many-to-Many relationship one or more rows from an entity are associated with one or more row in other entity. In JPA we define this using @ManyToMany annotation. Many to many relationship need a join table. We define the join table using the @JoinTable annotation. Join table contain foreign key to source and target object primary keys(joinColumns, inverseJoinColumns).



- Example 1  
  In Customer class:

  ```java
@ManyToMany
@JoinTable(name="CUST_PHONES")
public Set<PhoneNumber> getPhones() { return phones; }
```

  In PhoneNumber class:

  ```java
@ManyToMany(mappedBy="phones")
public Set<Customer> getCustomers() { return customers; }
```

- Example 2:  
  In Customer class:

  ```java
@ManyToMany(targetEntity=com.acme.PhoneNumber.class)
public Set getPhones() { return phones; }
```
In PhoneNumber class:

  ```java
@ManyToMany(targetEntity=com.acme.Customer.class, mappedBy="phones")
public Set getCustomers() { return customers; }
```

- Example 3:  
  In Customer class:

  ```java
@ManyToMany
@JoinTable(
name="CUST_PHONE",
joinColumns=
@JoinColumn(name="CUST_ID", referencedColumnName="ID"),
inverseJoinColumns=
@JoinColumn(name="PHONE_ID", referencedColumnName="ID")
)
public Set<PhoneNumber> getPhones() { return phones; }
```
In PhoneNumberClass:

  ```java
@ManyToMany(mappedBy="phones")
public Set<Customer> getCustomers() { return customers; }
```

- Example 4:
  Embeddable class used by the Employee entity specifies a many-to-many relationship.

  ```java
@Entity
public class Employee {
@Id int id;
@Embedded ContactInfo contactInfo;
...
}

@Embeddable
public class ContactInfo {
@ManyToOne Address address; // Unidirectional
@ManyToMany List<PhoneNumber> phoneNumbers; // Bidirectional
}

@Entity
public class PhoneNumber {
@Id int phNumber;
@ManyToMany(mappedBy="contactInfo.phoneNumbers")
Collection<Employee> employees;
}
```

### Eager and Lazy Loading
Eager loading is needed when we want relationship immediately fetched(eagerly), while Lazy loading is used when we want to load relationship later(lazily).
Usually used in Relationship annotation, and @Basic annotation. The FethType value is between EAGER or LAZY
Here is example usage.

```java
@OneToOne(fetch = FetchType.EAGER)
@OneToMany(fetch = FetchType.LAZY)
@ManyToOne(fetch = FetchType.EAGER)
@ManyToMany(fetch = FetchType.LAZY)
@Basic(fetch = FetchType.LAZY)
```

How to initialize lazy attribute?

1. Call a method on the mapped relation  
  To fetch Lazily attribute we just need to access the attribute using the accessor. Then Entity Manager will fetch those attribute using the relation using a query. Good for small amount entity relation, worse on huge amount of entity relation. Number of query executed equal to number of entity relation.
  
  ```java
Order order = this.em.find(Order.class, orderId);
order.getItems().size();
  ```
  
2. Use fetch join in JPQL query  
  To use fetch join in JPQL query add "JOIN FETCH" syntax to existing entity query. Good performance because only need to perform one query to fetch all needed relation, bad code maintenance if you have many use case with different fetch needed then you need to write specific query for specific need.
  
  Usage  
  ```java
class Order{
	...;
	@OneToMany(..);
	private Set<Item> items;
	
	...
}


Query q = this.em.createQuery("SELECT o FROM Order o JOIN FETCH o.items i WHERE o.id = :id");
q.setParameter("id", orderId);
newOrder = (Order) q.getSingleResult();
  ```
  
3. Use fetch join in criteria API  
  Almost same with JPQL in this approach we use criteria API to fetch lazy attributes. Performance and maintainability same with JPQL.  
  
  Usage

  ```java
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery q = cb.createQuery(Order.class);
Root o = q.from(Order.class);
o.fetch("items", JoinType.INNER);
q.select(o);
q.where(cb.equal(o.get("id"), orderId));

Order order = (Order)this.em.createQuery(q).getSingleResult();
  ```

4. Named Entity Graph
When using named Entity Graph we need to define @NamedEntityGraph to the entity class. In Order to Add the fields to the entity graph, we need to specifying them in the attributeNodes element of @NamedEntityGraph with a javax.persistence.NamedAttributeNode annotation. If wee want to define more than one NamedEntityGraph we can wrap them inside @NameEntityGraphs anotation.

  Usage:
  - Single Usage
  ```java
@NamedEntityGraph
@Entity
public class EmailMessage {
    @Id
    String messageId;
    String subject;
    String body;
    String sender;
}
```

  ```java
@NamedEntityGraph(name="emailEntityGraph", attributeNodes={
    @NamedAttributeNode("subject"),
    @NamedAttributeNode("sender")
})
@Entity
public class EmailMessage { ... }
```

  - Multiple usage

  ```java
@NamedEntityGraphs({
    @NamedEntityGraph(name="previewEmailEntityGraph", attributeNodes={
        @NamedAttributeNode("subject"),
        @NamedAttributeNode("sender"),
        @NamedAttributeNode("body")
    }),
    @NamedEntityGraph(name="fullEmailEntityGraph", attributeNodes={
        @NamedAttributeNode("sender"),
        @NamedAttributeNode("subject"),
        @NamedAttributeNode("body"),
        @NamedAttributeNode("attachments")
    })
})
@Entity
public class EmailMessage { ... }
```

  - Execution using find
  ```java
EntityGraph<EmailMessage> eg = em.getEntityGraph("emailEntityGraph");
Map hints = new HashMap();
hints.put("javax.persistence.fetchgraph", graph); // or javax.persistence.loadgraph

return this.em.find(Order.class, orderId, hints);
```

  - Execution using query
  
  ```java
EntityGraph<EmailMessage> eg = em.getEntityGraph("emailEntityGraph");
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery q = cb.createQuery(EmailMessage.class);
Root<EmailMessage> o = q.from(EmailMessage.class); // o is unused
TypedQuery<EmailMessage> q = em.createQuery(cq);
List<EmailMessage> emailMessage = (EmailMessage)this.em.createQuery(q).setHint("javax.persistence.loadgraph", eg).getResultList(); // or javax.persistence.fetchgraph
```

  https://docs.oracle.com/javaee/7/tutorial/persistence-entitygraphs002.htm

5. Dynamic Entity Graph
In Dynamic Entity Graph we don;t need to define the annotation in entity class. We can directly create the entity graph.

  Usage

  ```java
EntityGraph<EmailMessage> eg = em.createEntityGraph(EmailMessage.class);
eg.addAttributeNodes("body");
...
Properties props = new Properties();
props.put("javax.persistence.fetchgraph", eg); // or javax.persistence.loadgraph
EmailMessage message = em.find(EmailMessage.class, id, props);
```

  https://docs.oracle.com/javaee/7/tutorial/persistence-entitygraphs001.htm

Note:
- javax.persistence.fetchgraph will load all attributes defined in the EntityGraph, and all unlisted attributes will try to use LAZY load.
- javax.persistence.loadgraph will load all attributes defined in the EntityGraph, and all unlisted attributes will apply it's fetch settings.
- Entity Graph need JPA 2.1 and JPA 2.1 Compatible Provider

## Entity Managers
### Putting Entities to Work

### persistence.xml
When you want to create EntityManagerFactory from a persistence unit, you need to defined that persistence unit in persistence.xml

Java Usage:

```java
 EntityManagerFactory emfactory = Persistence.createEntityManagerFactory( "persistence_unit_name" );
      EntityManager entitymanager = emfactory.createEntityManager();
```
persistence.xml

```xml
...
 <persistence-unit name="persistence_unit_name" transaction-type="RESOURCE_LOCAL">
...
```
### Entity State and Transitions

### Managing Transactions
Transaction in Entity Manager operation can be controlled used JTA or resource-local EntityTransaction API.  
An entity manager whose underlying transactions are controlled through JTA is termed a JTA entity manager.  
An entity manager whose underlying transactions are controlled by the application through the EntityTransaction API is termed a resource-local entity manager.

Usage:

- Non-managed environment(Resource-Local)

Example:

```java
// Non-managed environment idiom
EntityManager em = emf.createEntityManager();
EntityTransaction tx = null;
try {
    tx = em.getTransaction();
    tx.begin();

    // do some work
    ...

    tx.commit();
}
catch (RuntimeException e) {
    if ( tx != null && tx.isActive() ) tx.rollback();
    throw e; // or display error message
}
finally {
    em.close();
}
```

- JTA container Managed Transaction(CMT)

Example:


```java
// CMT idiom
@Resource public EntityManagerFactory factory;

public void doBusiness() {
    EntityManager em = factory.createEntityManager();
    try {

    // do some work
    ...
}
catch (RuntimeException e) {
    throw e; // or display error message
}
finally {
    em.close();
}
```

Example
- JTA Bean managed Transaction(BMT)

Example:

```java
// BMT idiom
@Resource public UserTransaction utx;
@Resource public EntityManagerFactory factory;

public void doBusiness() {
    EntityManager em = factory.createEntityManager();
    try {

    // do some work
    ...

    utx.commit();
}
catch (RuntimeException e) {
    if (utx != null) utx.rollback();
    throw e; // or display error message
}
finally {
    em.close();
}
```

http://www.byteslounge.com/tutorials/container-vs-application-managed-entitymanager

### Persistence Operations
Persistence Operation cover load and store to Entity database(CRUD).
Usage Example in Java:

```java
EntityManagerFactory emfactory = Persistence.createEntityManagerFactory( "persistence_unit_name" );
      
EntityManager entitymanager = emfactory.createEntityManager( );
entitymanager.getTransaction( ).begin( );

Customer customerSample1 = new Customer();
customerSample1.setFullName("Anastasia Varah");
customerSample1.setGender("Female");
customerSample1.setPhoneNumber("+62080989999");
entitymanager.persist( employee ); //Create New
entitymanager.getTransaction( ).commit( );
...
Customer customerLoadSample = entitymanager.find( Customer.class, 1 ); //Find Entity

customerLoadSample.setPhoneNumber( "+62080989997" );  //Update Entity data
entitymanager.getTransaction( ).commit( ); // Commit Update

...
customerLoadSample customerLoadSampleToRemove= entitymanager.find( Customer.class, 1 ); //Find Entity
entitymanager.remove( customerLoadSampleToRemove ); //remove entity
entitymanager.getTransaction( ).commit( ); // commit remove

entitymanager.close( );
emfactory.close( );
```

### Creating Queries
Java Use Query interface for querying to database.
Here is easiest way to create Query.

```java
EntityManager em = getEntityManager();
jpqlStringQuery = "select emp from Employee emp";
Query query = em.createQuery(jpqlStringQuery);
```

### Named Queries
A named query is used for a static query that will be used many times in the application. The advantage of a named query is that it can be defined once, in one place, and reused in the application

```java

@NamedQuery(
  name="findAllEmployees",
  query="Select emp from Employee emp" //parameter is city
  hints={@QueryHint(name="acme.jpa.batch", value="emp.address")}
)
public class Employee {
  ...
}

EntityManager em = getEntityManager();
Query query = em.createNamedQuery("findAllEmployees"); //usage
List<Employee> employees = query.getResultList();
```

There is also @NamedNativeQuery if you want to use native SQL query

usage:

```java
@NamedNativeQuery(
  name="findAllEmployeesInCity",
  query="SELECT E.* from EMP E, ADDRESS A WHERE E.EMP_ID = A.EMP_ID AND A.CITY = ?",
  resultClass=Employee.class
)
public class Employee {
  ...
}
```
There first example used if you select only one entity and use select entity.* column. If more than one entity you can reference to sample below.

```java
@NamedNativeQuery(
  name="findAllEmployeesInCity",
  query="SELECT E.*, A.* from EMP E, ADDRESS A WHERE E.EMP_ID = A.EMP_ID AND A.CITY = ?",
  resultSetMapping="employee-address"
)
@SqlResultSetMapping(name="employee-address", 
  entities={ 
    @EntityResult(entityClass=Employee.class),
    @EntityResult(entityClass=Address.class)}
)
public class Employee {
  ...
}
```

If your named native query select partial column of an entity you need to map using @ContructorResult.

```java
@NamedNativeQuery(
  name="findAllEmployeeDetails",
  query="SELECT E.EMP_ID, E.F_NAME, E.L_NAME, A.CITY from EMP E, ADDRESS A WHERE E.EMP_ID = A.EMP_ID",
  resultSetMapping="employee-details"
)
@SqlResultSetMapping(name="employee-details", 
  classes={ 
    @ConstructorResult(targetClass=EmployeeDetails.class, columns={
        @ColumnResult(name="EMP_ID", type=Integer.class),
        @ColumnResult(name="F_NAME", type=String.class),
        @ColumnResult(name="L_NAME", type=String.class),
        @ColumnResult(name="CITY", type=String.class)
    })
  }
)
public class Employee {
  ...
}
```


### Query Parameters
To add query parameter in query string you can write ":" followed by you parameter name. To set the parameter value call setParameter method of Query interface.

```java

@NamedQuery(
  name="findAllEmployeesInCity",
  query="Select emp from Employee emp where emp.address.city = :city" //parameter is city
  hints={@QueryHint(name="acme.jpa.batch", value="emp.address")}
)
public class Employee {
  ...
}

EntityManager em = getEntityManager();
Query query = em.createNamedQuery("findAllEmployeesInCity");
query.setParameter("city", "Ottawa"); //set city value
List<Employee> employees = query.getResultList();
```
### Native Queries
To use native query we can simply call createNativeQuery() method from entity manager.

Sample 1:

```java
List<Author> results = this.em.createNativeQuery("SELECT a.id, a.firstName, a.lastName, a.version FROM Author a", Author.class).getResultList();
```

In first sample we need to define all Author column name correctly in order to auto map result to Author Entity. If we want custom mapping we need to define it in an xml file orm.xml.

```xml
<sql-result-set-mapping name="AuthorMappingXml">
    <entity-result entity-class="org.thoughts.on.java.jpa.model.Author">
        <field-result name="id" column="authorId"/>
        <field-result name="firstName" column="firstName"/>
        <field-result name="lastName" column="lastName"/>
        <field-result name="version" column="version"/>
    </entity-result>
</sql-result-set-mapping>
```

Now we can use it to mapping below query where id changed as authorId.

```java
List<Author> results = this.em.createNativeQuery("SELECT a.id as authorId, a.firstName, a.lastName, a.version FROM Author a", "AuthorMapping").getResultList();
```

##JPQL
### The Java Persistence Query Language
JPA provided SQL like way to query entity called JPQL. 

### Query Structure
JPQL query can retrieve an entity object rather than field result set from database, as with SQL. The JPQL query structure as follows.

```
SELECT ... FROM ...
[WHERE ...]
[GROUP BY ... [HAVING ...]]
[ORDER BY ...]
```

The structure of JPQL DELETE and UPDATE queries is simpler as follows.

```
DELETE FROM ... [WHERE ...]
 
UPDATE ... SET ... [WHERE ...]
```
### Path Expressions

### Filtering

### Scalar Functions
Numeric, string, datetime, case, and entity type expressions result in scalar values.

- Arithmetic
 - unary(+,-)
 - mupltipication and division(*,/)
 - addition and subtraction(+, -)
- Built-in String, Arithmetic, and Datetime Functional Expressions
 - String Function
  - CONCAT(string_expression, string_expression {, string_expression}* ) 
  - SUBSTRING(string_expression,arithmetic_expression [, arithmetic_expression]) 
  - TRIM([[trim_specification] [trim_character] FROM] string_expression) 
  - LOWER(string_expression) 
  - UPPER(string_expression)trim_specification ::= LEADING | TRAILING | BOTH
  - LENGTH(string_expression) 
  - LOCATE(string_expression, string_expression[, arithmetic_expression])
 - Arithmetic Functions
  - ABS(arithmetic_expression) 
  - SQRT(arithmetic_expression) 
  - MOD(arithmetic_expression, arithmetic_expression) 
  - SIZE(collection_valued_path_expression) 
  - INDEX(identification_variable)
 - Datetime Functions
  - CURRENT_DATE 
  - CURRENT_TIME 
  - CURRENT_TIMESTAMP
 - Invocation of Predefined and User-defined Database Functions
  - FUNCTION(function_name {, function_arg}*)
 - Case Expressions
  - CASE
  - WHEN
  - ELSE
  - END
  - COALESCE(scalar_expression {, scalar_expression}+)
 - Entity Type Expressions
  - TYPE (SELECT e FROM Employee e WHERE TYPE(e) IN (Exempt, Contractor))
  
### Operators and Precedence between, like, in, is null, is empty
### Ordering
### Aliases
### Grouping
### Aggregate Functions
### Joins
### Constructors

##Advanced Mapping
### Inheritance Strategies
### Single-Table Strategy
### Joined-Table Strategy
### Table-Per-Concrete-Class Strategy
### Querying Over Inheritance Relationships
### Secondary Tables
### Composite Primary Keys
### @IdClass and @EmbeddedId
### Derived Identifiers
### @ElementCollection
### Default Values
### @Version Fields
### Cascading and Orphan Removal
### Detachment and Merging

##The Criteria API
### History of the Criteria API
### Criteria Query Structure
### The MetaModel API and Query Type Safety
### Tuples
### Joins
### Predicates
### Building Expressions
### Ordering
### Grouping
### Encapsulating Persistence Logic
### Façades
### Range Queries


##Lifecycle and Validation
### Lifecycle Events
### Method Annotations
### Entity Listeners
### JSR-303 Validation
### Constraint Annotations
### Validation Modes
### Validation Groups

##Locking and Caching
### Concurrency
### Optimistic Locking
### Optimistic Read Locking
### Optimistic Write Locking
### Pessimistic Locking
### Caching
### Persistence Context as Transactional Cache
### Shared (2nd-level) Cache
### Locking and Caching "Do's and Don'ts"
