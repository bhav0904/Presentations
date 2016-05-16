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
MutableList<Person> people = Lists.mutable.of(person1, person2, person3);

MutableList<Address> addresses = people.collect(new Function<Person, Address>() 
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
MutableList<Person> people = Lists.mutable.of(person1, person2, person3);

MutableList<Address> addressesLambda = people.collect(person -> person.getAddress());

MutableList<Address> addressesMethodReference = people.collect(Person::getAddress);

```



