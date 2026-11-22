---
tags:
  - Spring
---
# @OneToOne
A one to one relationship is specified through two annotations:
```java
// In Parent Entity

@OneToOne
@JoinColumn(name = "child_id")
private Child child;
```
Here the `@JoinColumn`  references the `child_id` field in the child class. 
**The class that has the `@JoinColumn` is the parent class and is the one that owns the relationship**.
The above relationship is **uni-directional**.
## Bi-Directional One-To-One
To make a one-to-one relationship bi-directional another `@OneToOne` annotation needs to be added to the child class
```java
// In Child Entity

@OneToOne(mappedBy="child")
private Parent parent
```
The mapped by attributes tells JPA that child is not the owner of the relationship and the foreign key will go in the parent table. `mappedBy="child"` the value is the name of the child class in the parent class
# @OneToMany / @ManyToOne
Both of these relationships are related to each other.
The trick to figure out which annotation to use in what class is to take the first word and append the class name to it i.e:
- ***Many** People to **One** Address*
	- People has `@ManyToOne` annotation
		- `private Address address`
	- Address has `@OneToMany` annotation
		- `private List<People> peoples`
The owning is specified through the `@JoinColumn` and the `mappedBy` property similar to the `@OneToOne` relationship

```java
// In Parent Entity
    // One Parent -> Many Children
    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Child> children = new ArrayList<>();
```

```java
// In child entity
    // Many Children -> One Parent
    @ManyToOne
    @JoinColumn(name = "parent_id")
    private Parent parent;
```

**Note: You can still have a uni-directional relationship here as well as a bi-directional relationship**
**Note: There is no rule over which side should own the relationship but it makes more sense for it to be the class holding the `@ManyToOne` annotation**. Many people have one address. Here it makes more sense for the people to hold the foreign key to the address, as if the address was the one holding the foreign key to people, you would have multiple address records with the same data (postcode, street, etc) but a different foreign key. And every update would require N updates (N = number of people records associated with that address). The people holding the foreign key makes more sense.

# @ManyToMany
Many to Many relationships are handled by a join table between the two entites. 
One side will be the owner and it will include the `@JoinTable` annotation whilst the other will set the `mappedBy` property
```java
// Owner/Parent Entity

@ManyToMany
@JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private Set<Course> courses = new HashSet<>();
```

```java
// Child Entity

 @ManyToMany(mappedBy = "courses")   // Non-owning side, mapped by "courses" in Student
    private Set<Student> students = new HashSet<>();

```

# Uni-directional & Bi-directional
Uni-Directional: Relationship between entites goes one way. Cannot access entities from both sides
Bi-Direcitional: Relationships between entities goes both way. Can accesss entities from both sides