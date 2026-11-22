---
tags:
  - FORMAT
  - Spring
---
Spring MVC provides robust support for handling file uploads and downloads using `MultipartFile`. Below, we'll break it down into three key areas:

# Handling Multipart File Uploads in Spring MVC

Spring Boot makes it easy to handle multipart file uploads with `MultipartFile`.

## Setup: Enable Multipart Support in `application.properties`

```
spring.servlet.multipart.enabled=true
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=50MB
```

This configuration enables multipart file uploads and restricts file size to 10MB per file and 50MB per request.

## File Upload Controller

```java
@RestController
@RequestMapping("/files")
public class FileUploadController {
    
    private static final String UPLOAD_DIR = "uploads/";

    @PostMapping("/upload")
    public ResponseEntity<String> uploadFile(@RequestParam("file") MultipartFile file) {
        try {
            Path filePath = Paths.get(UPLOAD_DIR + file.getOriginalFilename());
            Files.createDirectories(filePath.getParent()); // Ensure directory exists
            Files.write(filePath, file.getBytes()); // Save file
            
            return ResponseEntity.ok("File uploaded successfully: " + file.getOriginalFilename());
        } catch (IOException e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                                 .body("Failed to upload file: " + e.getMessage());
        }
    }
}
```

### How it Works:

1. The `@PostMapping("/upload")` endpoint accepts a file using `MultipartFile`.  
2. It creates the necessary directories and writes the file to disk.  
3. It returns a success message or an error if something goes wrong.

---

# Handling File Downloads in Spring MVC

For file downloads, we need an endpoint that reads and serves the requested file.

## File Download Controller

```java
@RestController
@RequestMapping("/files")
public class FileDownloadController {

    private static final String UPLOAD_DIR = "uploads/";

    @GetMapping("/download/{fileName}")
    public ResponseEntity<Resource> downloadFile(@PathVariable String fileName) {
        try {
            Path filePath = Paths.get(UPLOAD_DIR + fileName);
            Resource resource = new UrlResource(filePath.toUri());

            if (!resource.exists() || !resource.isReadable()) {
                return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
            }

            return ResponseEntity.ok()
                    .contentType(MediaType.APPLICATION_OCTET_STREAM)
                    .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + fileName + "\"")
                    .body(resource);
        } catch (IOException e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
        }
    }
}
```

### How it Works:

1. The `@GetMapping("/download/{fileName}")` endpoint takes the file name as a path variable.  
2. It retrieves the file as a `Resource` and ensures it exists.  
3. It sets headers to prompt a download in the browser.

---

# Streaming Large Files Efficiently

When dealing with large files, reading them into memory can cause `OutOfMemoryError`. Instead, we should **stream** them.

## Streaming Large File Downloads

Instead of loading the entire file into memory, we use `ResponseEntity` with `InputStreamResource` for efficient streaming.

```java
@GetMapping("/stream/{fileName}")
public ResponseEntity<InputStreamResource> streamFile(@PathVariable String fileName) {
    try {
        Path filePath = Paths.get(UPLOAD_DIR + fileName);
        File file = filePath.toFile();

        if (!file.exists()) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }

        InputStreamResource resource = new InputStreamResource(new FileInputStream(file));

        return ResponseEntity.ok()
                .contentType(MediaType.APPLICATION_OCTET_STREAM)
                .contentLength(file.length())
                .header(HttpHeaders.CONTENT_DISPOSITION, "inline; filename=\"" + fileName + "\"")
                .body(resource);
    } catch (IOException e) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
    }
}
```

### Why This is Efficient:

* **Uses `InputStreamResource`**: Streams the file instead of loading it into memory.  
* **Supports large files**: Prevents memory exhaustion for files in gigabytes.

---

# Conclusion

* **File Upload:** Use `MultipartFile`, validate file sizes, and save securely.  
* **File Download:** Serve files as `Resource`, setting appropriate headers.  
* **Streaming Large Files:** Use `InputStreamResource` to avoid memory issues.

By following these best practices, you ensure efficient and scalable file handling in Spring MVC. 🚀

