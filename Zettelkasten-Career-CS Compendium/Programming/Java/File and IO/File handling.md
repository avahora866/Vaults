---
tags:
  - Java
---
Java has two file APIs.

- The original `java.io.File` API, available since Java 1.0 (1996).
- The newer `java.nio.file.Path` API, available since Java 1.7 (2011).

# What is the difference between the File and Path APIs?
The old file API is used in a ton of older projects, frameworks and libraries. Despite its age, it is not deprecated (and likely never will be) and you can still use it with any of the latest Java versions.
Nevertheless, `java.nio.file.Path` does everything `java.io.File` can, but generally in a better way and more. A few examples:

- File Features: The new classes support symlinks, proper file attributes and metadata support (think: `PosixFileAttributes`), ACLs and more.
- Better usage: E.g. when deleting a file, you get an exception with a meaningful error message (no such file, file locked, etc.), instead of a simple boolean saying false.
- Decoupling: Enabling support for in-memory file systems, which we’ll cover later.

# Path API
```java
public static void main(String[] args) throws URISyntaxException {

    // Java11+  : Path.of()
    Path path = Path.of("c:\\dev\\licenses\\windows\\readme.txt");
    System.out.println(path);

    path = Path.of("c:/dev/licenses/windows/readme.txt");
    System.out.println(path);

    path = Path.of("c:" , "dev", "licenses", "windows", "readme.txt");
    System.out.println(path);

    path = Path.of("c:" , "dev", "licenses", "windows").resolve("readme.txt"); // resolve == getChild()
    System.out.println(path);

    path = Path.of(new URI("file:///c:/dev/licenses/windows/readme.txt"));
    System.out.println(path);

    // Java < 11 equivalent: Paths.get()
    path = Paths.get("c:/dev/licenses/windows/readme.txt");
    System.out.println(path);

    // etc...
}
```

It does not matter if you are using forward slashes e.g. on Windows, as the Path API is smart enough to construct the right path, independently of the OS and any forward-backward slash issues.

Two important points, however:

1. Constructing a path object or resolving a child, does not mean the file or directory actually exists. The path is merely a reference to a potential file. So, you’ll have to separately verify its existence.
2. Pre Java-11, `Path.of` was called `Paths.get`, which you’ll need to use if you’re stuck on older Java versions or building a library that needs some backward compatibility. Starting with Java 11, `Paths.get` internally redirects to `Path.of`.

# Common File Operations
- `Files.exists(path)`
- `Files.getLastModifiedTime(path)`
- `Files.mismatch(path, Paths.get("c:\\dev\\whatever.txt"))`
	- This is a relatively new addition to Java, available since Java 12. It compares the sizes and bytes of two files and returns the position of the first (byte) mismatch. Or, -1L if there was no mismatch. Hence, if you are comparing two completely different files, you’ll get this as console output: the very first byte already didn’t match, hence the mismatch is position zero.
- `Files.getOwner(path)`
	- Self explanatory. Returns the owner of a file or directory as UserPrincipal (which extends from Principal). On Windows, this will be a WindowsUserPrincipal, which contains the user’s account name (shown below), as well as his sid, his unique security identifier on your Windows machine.
- Temp Files
	- `Files.createTempFile("somePrefixOrNull", ".jpg");`
		- When creating temp files, you can specify a prefix (first param) and a suffix (second param). Both can be null. The prefix will be prefixed (duh!) to the temp file name, the suffix is essentially the file extension, and if you leave it out a default extension of ".tmp" will be used. The file will be created in the default temporary-file directory.
	- `Files.createTempFile(path.getParent(), "somePrefixOrNull", ".jpg");`
		- Instead of the default temp directory, you can also specify your own directory where you want the temp file to be created.
	- `Files.createTempDirectory("prefix")`
		- In addition to files, you can also create temp directories. As you don’t need the suffix parameter when creating dirs, you only have to choice of specifying a prefix parameter.
	- `tempFile1 = C:\Users\marco\AppData\Local\Temp\somePrefixOrNull8747488053128491901.jpg
	- `tempFile2 = c:\dev\licenses\windows\somePrefixOrNull11086918945318459411.jpg`
	- `tmpDirectory = C:\Users\marco\AppData\Local\Temp\prefix9583768274092262832`
	- Note: Temp files, contrary to popular belief, do not delete themselves. You have to make sure to explicitly delete them, when creating them in unit tests or when running in production.
- Creating Files + Directories
	- `Files.createDirectories(path.getParent().resolve("some/new/dir"))`
	- `Files.createFile(newDirectory.resolve("emptyFile.txt"));`
	- Some people are confused by this: The .resolve call does not create the file, it merely returns a reference to the (child) file you are about to create.
	
- Posix permission of file
	- If you are running your Java program on a Unix-like system (including Linux and MacOS), you can get a file’s Posix permissions. Think: "-rw-rw-rw-" or "-rwxrwxrwx" etc.
	 ```java
	 Path path = Path.of("c:\\dev\\licenses\\windows\\readme.txt");
	try {
		Set<PosixFilePermission> permissions = Files.getPosixFilePermissions(path);
		System.out.println("permissions = " + permissions);
	} catch (UnsupportedOperationException e) {
		System.err.println("Looks like you're not running on a posix file system");
	 ```

# Writing and Reading to a File
## Writing strings to a file
```java
Path utfFile = Files.createTempFile("some", ".txt");
Files.writeString(utfFile, "this is my string ää öö üü"); // UTF 8
System.out.println("utfFile = " + utfFile);

Path iso88591File = Files.createTempFile("some", ".txt");
Files.writeString(iso88591File, "this is my string ää öö üü", StandardCharsets.ISO_8859_1); // otherwise == utf8
System.out.println("iso88591File = " + iso88591File);
```

## Writing bytes to a file
```java
Path anotherIso88591File = Files.createTempFile("some", ".txt");
Files.write(anotherIso88591File, "this is my string ää öö üü".getBytes(StandardCharsets.ISO_8859_1));
System.out.println("anotherIso88591File = " + anotherIso88591File);
```
If you want to write bytes to a file (and in older Java versions < 11 you’d have to use the same API for writing strings), you need to call `Files.write`.

When calling either of the write methods, the file will automatically be created (and truncated if it already exists). Which means, we wouldn’t have had to create explicit temporary files, like we did above.

If you don’t want that behaviour (i.e. fail if the file already exists) and get a corresponding exception, you’ll need to pass in another OpenOption.

## Using Writers and OutputStreams
```java
try (BufferedWriter bufferedWriter = Files.newBufferedWriter(utfFile)) {
    // handle reader
}

try (OutputStream os = Files.newOutputStream(utfFile)) {
    // handle outputstream
}
```
Last but not least, if you want to work directly with writers or output streams, make sure to call the corresponding Files methods and not construct the writers or streams by hand.

[[Writers, Readers and Streams]]

## Reading strings to a file
```java
String s = Files.readString(utfFile);// UTF 8
System.out.println("s = " + s);

s = Files.readString(utfFile, StandardCharsets.ISO_8859_1); // otherwise == utf8
System.out.println("s = " + s);
```
## Read bytes from a file
```java
s = new String(Files.readAllBytes(utfFile), StandardCharsets.UTF_8);
System.out.println("s = " + s);
```
If you want to read bytes from a file (and in older Java versions < 11 you’d have to use the same API for reading strings), you need to call Files.readAllBytes.

In case the final result should be a string, you’d then have to construct it yourself, with the appropriate encoding.

## Using Readers and Input Streams
```java
try (BufferedReader bufferedReader = Files.newBufferedReader(utfFile)) {
    // handle reader
}

try (InputStream is = Files.newInputStream(utfFile)) {
    // handle inputstream
}
```
As always, you can fall back to using readers or inputstreams directly. For that, use the corresponding Files methods.

[[Writers, Readers and Streams]]

# Moving, Deleting & Listing Files
## Moving
```java
Path utfFile = Files.createTempFile("some", ".txt");

try {
    Files.move(utfFile, Path.of("c:\\dev"));  // this is wrong!
} catch (FileAlreadyExistsException e) {
    // welp, that din't work!
}
```
There is a Files.move method, but it does not move a file to a designated directory (which you might expect).

- test.jpg → c:\temp does not work.
- test.jpg → c:\temp\test.jpg works.

`Files.move(utfFile, Path.of("c:\\dev").resolve(utfFile.getFileName().toString()));
`
So, you don’t move files to folders, but you "move" them to their full new path, including the filename and extension.

```java
Path utfFile2 = Files.createTempFile("some", ".txt");
Files.move(utfFile2, Path.of("c:\\dev").resolve(utfFile.getFileName().toString()), StandardCopyOption.REPLACE_EXISTING);

Path utfFile3 = Files.createTempFile("some", ".txt");
Files.move(utfFile3, Path.of("c:\\dev").resolve(utfFile.getFileName().toString()), StandardCopyOption.ATOMIC_MOVE);
```
When moving files, you can also specify how you want to move to happen, depending on the capabilities of the underlying file system.

- By default, if the target file already exists, a `FileAlreadyExistsException` will be thrown.
- If you specify the `StandardCopyOption.REPLACE_EXISTING` option, the target file will be overwritten.
- If you specify the `StandardCopyOption.ATOMIC_MOVE` option, you can move a file into a directory and be guaranteed that any process watching the directory accesses a complete file and not just a partial file.

## Deletion
```java
try {
    Files.delete(tmpDir);
} catch (DirectoryNotEmptyException e) {
    e.printStackTrace();
}
```
There is the `Files.delete` method, which allows you to delete files and directories, but directories only if they are empty.
There is unfortunately no flag to purge a non-empty directory, and you’ll simply get a `DirectoryNotEmptyException`.

### How to delete non-empty directories
There are some 3rd-party helper libraries to work around this, but if you want to use a plain Java version to delete a non-empty directory tree, this is what you’ll want to do:
```java
try (Stream<Path> walk = Files.walk(tmpDir)) {
    walk.sorted(Comparator.reverseOrder()).forEach(path -> {
        try {
            Files.delete(path);
        } catch (IOException e) {
            // something could not be deleted..
            e.printStackTrace();
        }
    });
}
```
`Files.walk` will walk a file tree depth-first, starting with the directory you specify. The reverseOrder comparator will make sure that you delete all children, before deleting the actual directory.
Unfortunately, you’ll also need to catch the `IOException`, when using `Files.delete` inside the forEach consumer. 

## Listing
There are various ways how you can list all files in a given directory. If you only want to list files on the same levels as the directory (not recursively deeper), you can use these two methods:

```java
try (var files = Files.list(tmpDirectory)) {
    files.forEach(System.out::println);
}

try (var files = Files.newDirectoryStream(tmpDirectory, "*.txt")) {
    files.forEach(System.out::println);
}
```
Note, that `newDirectoryStream` (as opposed to `Files.list`) does not return a `java.util.stream.Stream`. Instead, it returns a `DirectoryStream`, which is a class that got introduced in Java 1.7, before the release of the Streams API in Java 8.

It does, however, allow you to specify a glob pattern (like *.txt), which does the job for simple listings, and is maybe a bit easier to read than fumbling with real Streams and the corresponding filter methods.

Also note, that the streams returned by both methods must also be closed (e.g. with a try-with-resources statement), otherwise the JVM will keep the file handle on the directory open, which (on Windows) effectively locks it.
### How to list files recursively
If you want to recursively list all files of a file tree, you’ll need to employ the method we used for deleting directories: `Files.walk`.

```java
try (var files = Files.walk(tmpDirectory)) {
    files.forEach(System.out::println);
}
```
Note, that the stream returned by `Files.walk` must also be closed (e.g. with a `try-with-resources` statement), otherwise the JVM will keep the file handle on the directory open, which (on Windows) effectively locks it.

# Absolute, Relative & Canonical Files
## Relative paths
```java
Path p = Paths.get("./src/main/java/../resources/some.properties");
System.out.println("p.isAbsolute() = " + p.isAbsolute());
```
Here, you’re constructing a new path, based on the current directory (.), even including a (..) at some point. Hence, the path is relative to your current directory, and `path.isAbsolute` will return false.

`p.isAbsolute() = false`
## Absolute Paths
```java
Path p2 = p.toAbsolutePath();
System.out.println("p2 = " + p2);
System.out.println("p2.isAbsolute() = " + p2.isAbsolute());
```
When you call toAbsolutePath on the path, it will get converted to an..well…​absolute path, in my case containing `C:\dev\java-files`. Note, the absolute path still contains the dots, for current directory and upper-directory!

```java
p2 = C:\dev\java-file-article\.\src\main\java\..\resources\some.properties
p2.isAbsolute() = true
```
## Normalised paths
How to get rid of the dots? You’ll need to call normalize.
```java
Path p3 = p.normalize().toAbsolutePath();
System.out.println("p3 = " + p3);
System.out.println("p3.isAbsolute() = " + p3.isAbsolute());
```
This normalized, absolute path, is also what you could have called the canonical path.

```java
p3 = C:\dev\java-file-article\src\main\resources\some.properties
p3.isAbsolute() = true
```
## Relativizing Paths
Last but not least, you can also go the other way. Instead of making relative paths absolute, you can make absolute paths relative.

```java
Path relativizedPath = Paths.get("C:\\dev\\java-file-article\\").relativize(p3);
System.out.println("relativizedPath = " + relativizedPath);
```
You’re essentially saying, given a certain base path, what is the relative path of my current (absolute) path. You’ll get the following output:

`relativizedPath = src\main\resources\some.properties`
# Watching files and directories
Some projects need to watch directories for newly created (think: uploaded) files and do something with them. You have two popular choices, when it comes to watching for changes in a directory, in Java.

- Java Watchservice
- Apache Commons-IO
# In-Memory File Systems
Some developers assume that working with files always means you’ll actually have to write them to your disk.
During testing, this leads to creating a lot of temp files and directories and then having to make sure to delete them again.
But, with Java’s Path-API, there’s a much better way: In-Memory File Systems.
They let you write and read files, completely in-memory, without ever hitting your disk. Super-fast and a great fit for testing

There are two Java in-memory file systems that are worth looking at.
- Memory File System
- JimFS