# Cursor2POJO
A library for dynamically mapping an Android Cursor to Java Objects.

# Configuration

In order to use Cursor2POJO you are required to add annotations to your database persisted properties. 
Doing this decalres to C2P the respective mapping of each property to database column.

```java
public Foo {
  @C2PColumnInfo(name = "name_column")
  private String name;
  
  @C2PColumnInfo(name = "number_column")
  private int number;
  
  public Foo (String name, int number) {
    this.name = name;
    this.number = number;
  }
}
```

# Usage
In order to keep this library as generic as possible, it leaves the aquirement of the `Crusor` up to your program. 
C2P requires 3 parameters in order to execute
A. The Cursor object
B. The expected POJO to be returned
C. The structure of the return (currently either a List or single instance of the POJO)


The implementation for single POJO mapping is as follows:
```java
return (Foo) C2PMapper.execute(
                cursor,
                new ResultDescriber(
                        ResultDescriber.StructureType.list,
                        Foo.class
                )
        );
```


The implementation for List of POJO mapping is as follows:

```java
return (List) C2PMapper.execute(
                cursor,
                new ResultDescriber(
                        ResultDescriber.StructureType.list,
                        Foo.class
                )
        );
```

# Limitations
You are required to declare if you want a List or single object instance as due to Type erasure in Java you cannot request the type of object contained in a List through reflection.

Complex structures such as @Embedded and @TypeConverter from Android Room persistance are not currently supported.
