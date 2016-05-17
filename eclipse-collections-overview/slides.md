Eclipse Collections
===================


What is Eclipse Collections?
----------------------------
* An Open Source Collections Framework for Java
* Rich Lambda-Ready API
* Additional collection types not found in the JDK like Bags, Multmaps\




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
![EC Features Comparison](https://github.com/bhav0904/Presentations/blob/gh-pages/eclipse-collections-overview/GSC_Features.png)


How rich & lambda-ready is your iterable?
-----------------------------------------
![EC Features Comparison](https://github.com/bhav0904/Presentations/blob/gh-pages/eclipse-collections-overview/GSC_RichIterable.png)



Lambda-Ready API
-----------------

```
MutableList<Person> people = Lists.mutable.of(person1, person2, person3);
```

Selecting people whose age is greater than 18
```
MutableList<Person> adults = people.select(person -> person.getAge() > 18);
```

Sorting people by their last name 
```
MutableList<Person> sortedByName = people.toSortedListBy(person -> person.getLastName());
```
```
MutableList<Person> sortedByName = people.toSortedListBy(Person::getLastName);
```

Finding the oldest person
```
Person oldestPerson = people.maxBy(person -> person.getAge());
```
```
Person oldestPerson = people.maxBy(Person::getAge);
```



