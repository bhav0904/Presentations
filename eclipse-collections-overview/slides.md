Eclipse Collections
===================


What is Eclipse Collections?
----------------------------
* An Open Source Collections Framework for Java
* Rich Lambda-Ready API
* Additional collection types not found in the JDK like Bags, Multimaps and BiMaps




Code Example - Collect
------------------------------------
JDK 7 Collect
```
List<Person> people = Arrays.asList(person1, person2, person3);
List<Address> addresses = new ArrayList<>();

for (Person person : people) 
{
    addresses.add(people.getAddress());    
}
```


Code Example - Collect
------------------------------------
Eclipse Collections Collect With JDK 7
```
MutableList<Person> people = 
    Lists.mutable.of(person1, person2, person3);

MutableList<Address> addresses = 
people.collect(new Function<Person, Address>() 
               {
                    public Address valueOf(Person person) 
                    {
                        return person.getAddress();
               }
               });
```


Code Example - Collect
------------------------------------
Eclipse Collections Collect With JDK 8 
```
MutableList<Person> people = 
    Lists.mutable.of(person1, person2, person3);

MutableList<Address> addressesLambda = 
    people.collect(person -> person.getAddress());

MutableList<Address> addressesMethodReference = 
    people.collect(Person::getAddress);

```


Comparison with other Java Collections Frameworks
--------------------------------------------------
![EC Features Comparison](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/GSC_Features.png)



How rich & lambda-ready is your iterable?
-----------------------------------------
![EC Features Comparison](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/GSC_RichIterable.png)



Lambda-Ready API
-----------------

```
MutableList<Person> people = 
    Lists.mutable.of(person1, person2, person3);
```

Selecting people whose age is greater than 18
```
MutableList<Person> adults = 
    people.select(person -> person.getAge() > 18);
```

Sorting people by their last name 
```
MutableList<Person> sortedByName = 
    people.toSortedListBy(person -> person.getLastName());
```
```
MutableList<Person> sortedByName = people.toSortedListBy(Person::getLastName);
```

Finding the oldest person
```
Person oldestPerson = 
    people.maxBy(person -> person.getAge());
```
```
Person oldestPerson = 
    people.maxBy(Person::getAge);
```


The "with" Methods & Java 8 Method References
---------------------------------------------
```
MutableList<Person> adults = people.select(person -> person.olderThan(18));
```
```
MutableList<Person> drivers = people.selectWith(Person::olderThan, 16);

MutableList<Person> voters = people.selectWith(Person::olderThan, 18);

MutableList<Person> drinkers = people.selectWith(Person::olderThan, 21);
```



Lambda-Ready API - groupBy
---------------------------
Grouping people by state
```
Multimap<String,Person> peopleByState = 
    people.groupBy(person -> person.getState());
```
```
Multimap<String,Person> peopleByState = 
    people.groupBy(Person::getState);
```
![People By State](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/GSC_PeopleByState.png)

```
MutableListMultimap<String,Person> peopleByState = 
    people.groupBy(Person::getState);
```
```
MutableList<Person> newYorkers = 
    peopleByState.get(“NY");
```




Lambda-Ready API - groupByEach
-------------------------------
* Problem Statement: Every person has multiple addresses, each in a different state.
* Group people by state where a person appears as a value mapped to **every** state she has an address in.

![People By State](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/GSC_PeopleByStates.png)

Plain JDK 8 (no iteration pattern)
```
Map<String, List<Person>> peopleByStates = new HashMap<>();

for (Person person : people) 
{
    MutableList<String> states = person.getStates();

    for (String state : states) {
        if (peopleByStates.get(state) == null)
        {
            peopleByStates.put(state, new ArrayList<>());
        }
        peopleByStates.get(state).add(person);
    }
}
return peopleByStates;
```


Eclipse Collections groupByEach

```
Multimap<String, Person> peopleByStates = people.groupByEach(Person::getStates);
```

Memory Optimization
====================


Memory Optimization
-----------------------
* UnifiedMap - built wihtout using Entry objects.
* UnifiedSet - not built using a map.
* Empty should be empty.
* Primitive Collections.
* Memory-efficient containers for small-sized collections.


UnifiedMap
----------
* For every put, HashMap creates an Entry object.
* UnifiedMap stores keys and values in alternate slots on a single array.
* Consecutive memory locations are faster to access.


Save 50% Memory with the EC UnifiedMap
---------------------------------------
![UnifiedMap Memory](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/UnifiedMap.png)


UnifiedSet
----------
* HashSet uses HashMap as its backing collection.
* For every add, an Entry object is created with the element as key and null value.
* UnifiedSet uses an array as its backing collection.


Save 400% Memory with the EC UnifiedSet
---------------------------------------
![UnifiedSet Memory](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/UnifiedSet.png)


Primitive Collections
----------------------

* What are primitives?
    * Not everything in java is an Object.
    * Primitive types are automatic variables that are not references.
    * The variables hold the value and its place on the stack, so it’s much more efficient.
* Why primitive collections?
    * Reduced memory usage
    * Improved performance
    * Eliminates the need to depend on multiple libraries – PCJ, Trove etc.
* What primitive collections are available in EC?
    * List, Set, Map (all primitive/object combinations)
    * Stack
    * Bag
* For what all primitive types? 
    * All eight: boolean, byte, char, double, float, int, long, short


Save Memory with Primitive Collections: IntList
------------------------------------------------
![IntArrayList Memory](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/IntArrayList.png)



Resources
----------
* https://www.eclipse.org/collections/
* Eclipse Collections @ github : https://github.com/eclipse/eclipse-collections
* Eclipse Collections Kata : https://github.com/eclipse/eclipse-collections-kata
    * Fun way to learn idiomatic Eclipse Collections usage
* Eclipse Collectins wiki: https://github.com/eclipse/eclipse-collections/wiki
    *  Slides and videos of previous conference talks
    *  Articles


Are you up for the challenge?
----------------------------------
* Contribute to a code base with over 98% test coverage and strict code quality standards.
* Learn the usage of Java 8 lambdas (the unit tests are written in Java 8)
* Learn code generation using StringTemplate when you contribute to primitive collections.
* Roadmap available on github: https://github.com/eclipse/eclipse-collections/wiki/Roadmap



Appendix
==========


Java 8 Stream
--------------

* A sequence of elements from a source that supports aggregate operations.
* In Java 8, the JCF collections have a method stream()
```
default Stream<E> stream() 
{
    return StreamSupport.stream(spliterator());
}
```
* Streams support internal iterations and pipelining.
```
Stream<Address> stream = 
    people.stream().map(Person::getAddress);
```


Streams are Lazy
------------------
![EagerVsLazy](https://raw.githubusercontent.com/bhav0904/Presentations/gh-pages/eclipse-collections-overview/EagerVsLazy.png)




(meat vs bun slide)



