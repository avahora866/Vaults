---
tags:
  - Java
---
Java 8 introduced new APIs for *Date* and *Time* to address the shortcomings of the older *java.util.Date* and *java.util.Calendar*.

In this tutorial, let’s start with the issues in the existing *Date* and *Calendar* APIs and discuss how the new Java 8 *Date* and *Time* APIs address them.

We will also look at some of the core classes of the new Java 8 project that are part of the *java.time* package, such as *LocalDate*, *LocalTime*, *LocalDateTime*, *ZonedDateTime*, *Period*, *Duration* and their supported APIs.

# Issues With the Existing Date/Time APIs[](https://www.baeldung.com/java-8-date-time-intro#issues)

- **Thread safety** – The *Date* and *Calendar* classes are not thread safe, leaving developers to deal with the headache of hard-to-debug concurrency issues and to write additional code to handle thread safety. On the contrary, the new *Date* and *Time* APIs introduced in Java 8 are immutable and thread safe, thus taking that concurrency headache away from developers.
- **API design and ease of understanding** – The *Date* and *Calendar* APIs are poorly designed with inadequate methods to perform day-to-day operations. The new *Date*/*Time* API is ISO-centric and follows consistent domain models for date, time, duration and periods. There are a wide variety of utility methods that support the most common operations.
- ***ZonedDate* and *Time*** – Developers had to write additional logic to handle time-zone logic with the old APIs, whereas with the new APIs, handling of time zone can be done with *Local* and *ZonedDate*/*Time* APIs.

# LocalDate, LocalTime and LocalDateTime[](https://www.baeldung.com/java-8-date-time-intro#localDate)

The most commonly used classes are *LocalDate*, *LocalTime* and *LocalDateTime*. As their names indicate, they represent the local date/time from the context of the observer.

We mainly use these classes when time zones are not required to be explicitly specified in the context. As part of this section, we will cover the most commonly used APIs.

## Working With LocalDate[](https://www.baeldung.com/java-8-date-time-intro#1-working-with-localdate)

The *LocalDate* represents **a date in ISO format (yyyy-MM-dd) without time.** We can use it to store dates like birthdays and paydays.

An instance of current date can be created from the system clock:

```
LocalDate localDate = LocalDate.now();
```
And we can get the LocalDate representing a specific day, month and year by using the of method or the parse method.

For example, these code snippets represent the LocalDate for February 20, 2015:

```
LocalDate.of(2015, 02, 20);

LocalDate.parse("2015-02-20");
```
The LocalDate provides various utility methods to obtain a variety of information. Let’s have a quick peek at some of these API methods.

The following code snippet gets the current local date and adds one day:

```
LocalDate tomorrow = LocalDate.now().plusDays(1);
```
This example obtains the current date and subtracts one month. Note how it accepts an enum as the time unit:

```
LocalDate previousMonthSameDay = LocalDate.now().minus(1, ChronoUnit.MONTHS);
```
In the following two code examples, we parse the date “2016-06-12” and get the day of the week and the day of the month respectively. Note the return values — the first is an object representing the DayOfWeek, while the second is an int representing the ordinal value of the month:

```
DayOfWeek sunday = LocalDate.parse("2016-06-12").getDayOfWeek();

int twelve = LocalDate.parse("2016-06-12").getDayOfMonth();
```
We can test if a date occurs in a leap year, for example the current date:

```
boolean leapYear = LocalDate.now().isLeapYear();
```
Also, the relationship of a date to another can be determined to occur before or after another date:
```
boolean notBefore = LocalDate.parse("2016-06-12")
  .isBefore(LocalDate.parse("2016-06-11"));

boolean isAfter = LocalDate.parse("2016-06-12")
  .isAfter(LocalDate.parse("2016-06-11"));
  ```
  Finally, date boundaries can be obtained from a given date.

In the following two examples, we get the LocalDateTime that represents the beginning of the day (2016-06-12T00:00) of the given date and the LocalDate that represents the beginning of the month (2016-06-01) respectively:
```
LocalDateTime beginningOfDay = LocalDate.parse("2016-06-12").atStartOfDay();
LocalDate firstDayOfMonth = LocalDate.parse("2016-06-12")
  .with(TemporalAdjusters.firstDayOfMonth());
  ```
 ## Working With LocalTime
The LocalTime represents time without a date.

Similar to LocalDate, we can create an instance of LocalTime from the system clock or by using parse and of methods.

We’ll now take a quick look at some of the commonly used APIs.

An instance of current LocalTime can be created from the system clock:

```
LocalTime now = LocalTime.now();
```
We can create a LocalTime representing 6:30 a.m. by parsing a string representation:
```
LocalTime sixThirty = LocalTime.parse("06:30");
```
The factory method of can also be used to create a LocalTime. This code creates LocalTime representing 6:30 a.m. using the factory method:

```
LocalTime sixThirty = LocalTime.of(6, 30);
```
Let’s create a LocalTime by parsing a string and adding an hour to it by using the “plus” API. The result would be LocalTime representing 7:30 a.m.:
```
LocalTime sevenThirty = LocalTime.parse("06:30").plus(1, ChronoUnit.HOURS);
```
Various getter methods are available that can be used to get specific units of time like hour, min and secs:

```
int six = LocalTime.parse("06:30").getHour();
```
We can also check if a specific time is before or after another specific time. This code sample compares two LocalTime for which the result would be true:

```
boolean isbefore = LocalTime.parse("06:30").isBefore(LocalTime.parse("07:30"));
```
Finally, the max, min and noon time of a day can be obtained by constants in LocalTime class. This is very useful when performing database queries to find records within a given span of time.

For example, the below code represents 23:59:59.99:

```
LocalTime maxTime = LocalTime.MAX
```
## Working With LocalDateTime
LocalDateTime is used to represent a combination of date and time. This is the most commonly used class when we need a combination of date and time.

The class offers a variety of APIs. Here, we’ll look at some of the most commonly used ones.

An instance of LocalDateTime can be obtained from the system clock similar to LocalDate and LocalTime:

```
LocalDateTime.now();
```
The below code samples explain how to create an instance using the factory “of” and “parse” methods. The result would be a LocalDateTime instance representing February 20, 2015, 6:30 a.m.:

```
LocalDateTime.of(2015, Month.FEBRUARY, 20, 06, 30);
LocalDateTime.parse("2015-02-20T06:30:00");
```
There are utility APIs to support addition and subtraction of specific units of time like days, months, years and minutes.

The below code demonstrates the “plus” and “minus” methods. These APIs behave exactly like their counterparts in LocalDate and LocalTime:
```
localDateTime.plusDays(1);
localDateTime.minusHours(2);
```
Getter methods are also available to extract specific units similar to the date and time classes. Given the above instance of LocalDateTime, this code sample will return the month February:

```
localDateTime.getMonth();
```
## ZonedDateTime API
Java 8 provides ZonedDateTime when we need to deal with time-zone-specific date and time. The ZoneId is an identifier used to represent different zones. There are about 40 different time zones, and the ZoneId represents them as follows.

Here, we create a Zone for Paris:

```
ZoneId zoneId = ZoneId.of("Europe/Paris");
```
And we can get a set of all zone ids:
```
Set<String> allZoneIds = ZoneId.getAvailableZoneIds();
```
The LocalDateTime can be converted to a specific zone:
```
ZonedDateTime zonedDateTime = ZonedDateTime.of(localDateTime, zoneId);
```
The ZonedDateTime provides the parse method to get time-zone-specific date-time:
```
ZonedDateTime.parse("2015-05-03T10:15:30+01:00[Europe/Paris]");
```
Another way to work with time zone is by using OffsetDateTime. The OffsetDateTime is an immutable representation of a date-time with an offset. This class stores all date and time fields, to a precision of nanoseconds, as well as the offset from UTC/Greenwich.

The OffSetDateTime instance can be created using ZoneOffset. Here, we create a LocalDateTime representing 6:30 a.m. on February 20, 2015:
```
LocalDateTime localDateTime = LocalDateTime.of(2015, Month.FEBRUARY, 20, 06, 30);
```
Then we add two hours to the time by creating a ZoneOffset and setting for the localDateTime instance:
```
ZoneOffset offset = ZoneOffset.of("+02:00");

OffsetDateTime offSetByTwo = OffsetDateTime
  .of(localDateTime, offset);
```
We now have a localDateTime of 2015-02-20 06:30 +02:00.

Now let’s move on to how to modify date and time values using the Period and Duration classes.

# Period and Duration
The Period class represents a quantity of time in terms of years, months and days, and the Duration class represents a quantity of time in terms of seconds and nanoseconds.
## Working With Period
The Period class is widely used to modify values of given a date or to obtain the difference between two dates:

```
LocalDate initialDate = LocalDate.parse("2007-05-10");
```
We can manipulate the Date by using Period:

```
LocalDate finalDate = initialDate.plus(Period.ofDays(5));
```
The Period class has various getter methods such as getYears, getMonths and getDays to get values from a Period object.

For example, this returns an int value of 5 as we try to get difference in terms of days:

```
int five = Period.between(initialDate, finalDate).getDays();
```
We can get the Period between two dates in a specific unit such as days or months or years, using ChronoUnit.between:

```
long five = ChronoUnit.DAYS.between(initialDate, finalDate);
```
This code example returns five days.

Let’s continue by taking a look at the Duration class.
## Working With Duration
Similar to Period, the Duration class is used to deal with Time.

Let’s create a LocalTime of 6:30 a.m. and then add a duration of 30 seconds to make a LocalTime of 6:30:30 a.m.:
```
LocalTime initialTime = LocalTime.of(6, 30, 0);

LocalTime finalTime = initialTime.plus(Duration.ofSeconds(30));
```

We can get the Duration between two instants as either a Duration or a specific unit.

First, we use the between() method of the Duration class to find the time difference between finalTime and initialTime and return the difference in seconds:
```
long thirty = Duration.between(initialTime, finalTime).getSeconds();
```
In the second example, we use the between() method of the ChronoUnit class to perform the same operation:

```
long thirty = ChronoUnit.SECONDS.between(initialTime, finalTime);

```
Now we’ll look at how to convert existing Date and Calendar to new Date/Time.
# Compatibility With Date and Calendar
Java 8 has added the toInstant() method, which helps to convert existing Date and Calendar instance to new Date and Time API:
```
LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
LocalDateTime.ofInstant(calendar.toInstant(), ZoneId.systemDefault());
```
The LocalDateTime can be constructed from epoch seconds. The result of the below code would be a LocalDateTime representing 2016-06-13T11:34:50:

```
LocalDateTime.ofEpochSecond(1465817690, 0, ZoneOffset.UTC);
```
Now let’s move on to Date and Time formatting.

# Date and Time Formatting
Java 8 provides APIs for the easy formatting of Date and Time:

```
LocalDateTime localDateTime = LocalDateTime.of(2015, Month.JANUARY, 25, 6, 30);
```
This code passes an ISO date format to format the local date, with a result of 2015-01-25:
```
String localDateString = localDateTime.format(DateTimeFormatter.ISO_DATE);
```
The DateTimeFormatter provides various standard formatting options.

Custom patterns can be provided to the format method as well, which here returns a LocalDate as 2015/01/25:
```
localDateTime.format(DateTimeFormatter.ofPattern("yyyy/MM/dd"));
```
We can pass in formatting style either as SHORT, LONG or MEDIUM as part of the formatting option.

For example, this would give an output representing LocalDateTime in 25-Jan-2015, 06:30:00:
```
localDateTime
  .format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM)
  .withLocale(Locale.UK));
```

# Joda-Time Library
Another alternative for Java 8 Date and Time library is Joda-Time library. In fact, the Java 8 Date/Time API has been led jointly by the author of Joda-Time library (Stephen Colebourne) and Oracle. This library provides pretty much all capabilities that are supported in the Java 8 Date/Time project.

The artifact can be found in Maven Central by including the below pom dependency in our project:
```
<dependency>
    <groupId>joda-time</groupId>
    <artifactId>joda-time</artifactId>
    <version>2.9.4</version>
</dependency>
```
# java.sql.date
A java.sql.Date is merely a java.util.Date with its time set to 00:00:00.

# Databases
Since Java 8, the new Java Date and Time API is available for dealing with temporal values. This API fixes many of the problems of java.util.Date and java.util.Calendar classes.

The types from the java.time package are directly mapped to corresponding SQL types.

So, there’s no need to explicitly specify @Temporal annotation:

LocalDate is mapped to DATE.
LocalTime and OffsetTime are mapped to TIME.
Instant, LocalDateTime, OffsetDateTime and ZonedDateTime are mapped to TIMESTAMP.
This means that we can mark these fields only with @Basic (or @Column) annotation:

```
@Basic
private java.time.LocalDate localDate;

@Basic
private java.time.LocalTime localTimeField;

@Basic
private java.time.OffsetTime offsetTime;

@Basic
private java.time.Instant instant;

@Basic
private java.time.LocalDateTime localDateTime;

@Basic
private java.time.OffsetDateTime offsetDateTime;

@Basic
private java.time.ZonedDateTime zonedDateTime;
```
Every temporal class in the java.time package has a static parse() method to parse the provided String value using the appropriate format.

So, here’s how we can set the values of the entity fields:

```
temporalValues.setLocalDate(LocalDate.parse("2017-11-15"));

temporalValues.setLocalTime(LocalTime.parse("15:30:18"));
temporalValues.setOffsetTime(OffsetTime.parse("08:22:12+01:00"));

temporalValues.setInstant(Instant.parse("2017-11-15T08:22:12Z"));
temporalValues.setLocalDateTime(
  LocalDateTime.parse("2017-11-15T08:22:12"));
temporalValues.setOffsetDateTime(
  OffsetDateTime.parse("2017-11-15T08:22:12+01:00"));
temporalValues.setZonedDateTime(
  ZonedDateTime.parse("2017-11-15T08:22:12+01:00[Europe/Paris]"));
```
