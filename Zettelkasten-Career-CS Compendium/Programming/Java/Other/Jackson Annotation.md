---
tags:
  - Java
---

------------------------------------------------------------------------------
![Youtube link](https://www.youtube.com/watch?v=jMcNylLB-EA&list=PLO0KWyajXMh5n1_N5z1i1Y-6v8j2vfRXL&index=2)
# @JSON Property

Field level annotation.
Replaces the name of the field in the JSON object.
Can also be used to make a field read only

# @JSON Ignore

Ignores a field in JSON object
@JSON ignore properties can be put on the class, and you can set multiple fields to be ignored

# @JSON Property Order

Sets the order of the property when serialized (becoming a JSON object)

# @JSON Raw Value

Put on a field to allow the value to be a raw value of JSON

# @JSON Include

put on the class with a parameter of `Include.NON_EMPTY`, which makes it so if any of the fields are null they are removed from the JSON object during the serialization process

ark style="background: #FF5582A6;">### @JSON Value ???</mark>

Can allow for a method in ENUM class to accept value of Enum instead of type of Enum

# @JSON Enum Default Value

Allows you to add a default value to Enum which will be used whenever other values of Enum are not being used

ark style="background: #FF5582A6;">### @JSON Creator ??? //something to do with ENUM and previous annotation when not using spring</mark>

# @JSON Type

Brings in polymorphic behaviour with the use of annotations

Abstract class called vehicle, which has the annotation @JsonTypeInfo and @JsonSubTypes. @JsonSubTypes is the annotation that will define the subclasses, and @JsonTypeInfo defines the abstract class as the main class. @JsonTypeName is put on the subclasses to match with the subclass given in the abstract class

# @JSON Getter and @JSON Setter

These are put on the Boolean fields getter and setter as during serialization the getters and setters don't work here (Why???) so putting these annotations allow you to set and get the field

# @JsonAnyGetter and @JsonAnySetter

Allows you to map fields as standard properties.

`Map<String, String>`

Instead of the above being listed as a nested object, the values of the above will be listed as separate fields in the JSON object

# @JsonFormat

Used when using a date.
Put on a field with a type of date