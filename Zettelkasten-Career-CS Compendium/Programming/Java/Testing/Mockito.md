---
tags:
  - Java
---
Mockito is a java based mocking framework, used in conjunction with other testing frameworks such as JUnit and TestNG. It internally uses Java Reflection API and allows to create objects of a service. A mock object returns a dummy data and avoids external dependencies. It simplifies the development of tests by mocking external dependencies and apply the mocks into the code under test.

# Mockito Maven Dependencies
To implement Mockito based test cases in a project, add the following dependency to the pom.xml file of the project:
```xml
<dependency>
     <groupId>org.mockito</groupId>
     <artifactId>mockito-core</artifactId>
     <version>2.19.0</version>
     <scope>test</scope>
</dependency>
<dependency>
     <groupId>org.mockito</groupId>
     <artifactId>mockito-junit-jupiter</artifactId>
     <version>2.19.0</version>
     <scope>test</scope>
</dependency>
```
Note that mockito-junit-jupiter is required for JUnit 5, if you are using any other testing framework such as JUnit 4 or TestNG then you remove this dependency and include only mockito-core dependency.
# Mockito Mock Creation
The Mockito framework allows us to create mock objects using either @Mock annotation or mock() static method.

## Mockito mock() Method
The below example shows the usage of mock() method:
```java
package com.journaldev.mockito;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

import com.journaldev.AddService;
import com.journaldev.CalcService;

public class CalcService1Test {

	@Test
	void testCalc() {
		System.out.println("**--- Test testCalc executed ---**");

		AddService addService;
		CalcService calcService;

		addService = Mockito.mock(AddService.class);
		calcService = new CalcService(addService);

		int num1 = 11;
		int num2 = 12;
		int expected = 23;

		when(addService.add(num1, num2)).thenReturn(expected);

		int actual = calcService.calc(num1, num2);

		assertEquals(expected, actual);

	}
}
```
In the above example, we are testing CalcService. Mockito.mock() method is used to create a mock object of AddService class.
## Mockito Mock Annotation
The below example shows the usage of @Mock annotation.
```java
package com.journaldev.mockito;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

import com.journaldev.AddService;
import com.journaldev.CalcService;

public class CalcService2Test {

	CalcService calcService;

	@Mock
	AddService addService;

	@BeforeEach
	public void setup() {
		MockitoAnnotations.initMocks(this);
	}

	@Test
	public void testCalc() {
		System.out.println("**--- Test testCalc executed ---**");

		calcService = new CalcService(addService);

		int num1 = 11;
		int num2 = 12;
		int expected = 23;

		when(addService.add(num1, num2)).thenReturn(expected);

		int actual = calcService.calc(num1, num2);

		assertEquals(expected, actual);

	}
}
```
Note that we need to call MockitoAnnotations.initMocks(this); to initialize objects annotated with @Mock, @Spy, @Captor, or @InjectMocks.

## Mockito Behavior Verification
To add a behavior to the mocked class when() and thenReturn() functions are used. It means that when the mock object (addService) is called for add method with (num1, num2) parameters, then it returns the value stored in the expected variable. Our CalcService class looks like below:
```java
public class CalcService {
	
	private AddService addService;
	
	public CalcService(AddService addService) {
		this.addService = addService;
	}

	public int calc(int num1, int num2) {
		System.out.println("**--- CalcService calc executed ---**");
		return addService.add(num1, num2);
	}

}
```
The CalcService has a dependency on AddService class. It uses the AddService class’s add method to perform its operation. Since we wanted to do unit testing of CalcService class only, we have to mock the AddService instance. The AddService looks like below:

```java
public interface AddService {
	public int add(int num1, int num2);
}
```
```java
public class AddServiceImpl implements AddService {
	@Override
	public int add(int num1, int num2) {
		System.out.println("**--- AddServiceImpl add executed ---**");
		return num1 + num2;
	}
}
```
## Mockito Verify Interaction
Mockito framework keeps track of all the method calls and their parameters to the mock object. Mockito verify() method on the mock object verifies that a method is called with certain parameters. We can also specify the number of invocation logic, such as the exact number of times, at least specified number of times, less than the specified number of times, etc. We can use VerificationModeFactory for number of invocation times logic. Mockito verify() method checks that a method is called with the right parameters. It does not check the result of a method call like assert method. The below example demonstrates the usage of verify() method:
```java
package com.journaldev.mockito;

import static org.mockito.Mockito.verify;

import java.util.List;

import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.mockito.internal.verification.VerificationModeFactory;

public class VerifyInteractionTest {
	@Test
	public void testMethod() {
		@SuppressWarnings("unchecked")
		List<String> mockedList = Mockito.mock(List.class);

		mockedList.add("first-element");
		mockedList.add("second-element");
		mockedList.add("third-element");
		mockedList.add("third-element");
		mockedList.clear();

		verify(mockedList).add("first-element");
		verify(mockedList).add("second-element");
		verify(mockedList, VerificationModeFactory.times(2)).add("third-element");

		verify(mockedList).clear();
	}

}
```
## Mockito Stub Concrete Class
Using when() - thenReturn() function, we can stub a concrete/implementation class and also a single element of a collection. The non-stubbed elements will contains null in them.
```java
package com.journaldev.mockito;

import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.when;

import java.util.ArrayList;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class MockSingleElementTest {
	@SuppressWarnings("unchecked")
	@Test
	public void testMethod() {
		ArrayList mockedList = mock(ArrayList.class);

		when(mockedList.get(0)).thenReturn("first-element");

		System.out.println(mockedList.get(0));
		assertEquals("first-element", mockedList.get(0));
		
		// "null" gets printed as get(1) is not stubbed
		System.out.println(mockedList.get(1));
	}

}
```
## Mockito Spy
When you call the method of a spied object, the real method will be called, unless a predefined behavior was defined. Using spy we can define behavior by using when() - theReturn() functions or can invoke real implementation.
```java
package com.journaldev.mockito;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.spy;
import static org.mockito.Mockito.when;

import java.util.ArrayList;
import java.util.List;

import org.junit.jupiter.api.Test;

public class MockitoSpyTest {

	@Test
	public void testMethod() {
		List<String> list = new ArrayList<>();
		List<String> listSpy = spy(list);

		listSpy.add("first-element");
		System.out.println(listSpy.get(0));

		assertEquals("first-element", listSpy.get(0));
		when(listSpy.get(0)).thenReturn("second-element");
		System.out.println(listSpy.get(0));
		assertEquals("second-element", listSpy.get(0));
	}

```
