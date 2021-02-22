# HttpShuttle

[![Total alerts](https://img.shields.io/lgtm/alerts/g/jh3nd3rs0n/httpshuttle.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/jh3nd3rs0n/httpshuttle/alerts/) [![Language grade: Java](https://img.shields.io/lgtm/grade/java/g/jh3nd3rs0n/httpshuttle.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/jh3nd3rs0n/httpshuttle/context:java) [![Codacy Badge](https://app.codacy.com/project/badge/Grade/2ee3f3d4617f4ac989dd7cc21bc7e6fe)](https://www.codacy.com/gh/jh3nd3rs0n/httpshuttle/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=jh3nd3rs0n/httpshuttle&amp;utm_campaign=Badge_Grade)

HttpShuttle is a Java library of HTTP transport types.

## Contents

-   [Examples](#examples)
-   [Requirements](#requirements)
-   [Generating Javadocs](#generating-javadocs)
-   [Automated Testing](#automated-testing)
-   [Installing](#installing)
-   [Building](#building)

## Examples

```java
    
    // As a client
    
    // inputStream and outputStream are initialized outside this example    
    
    MessageHeader<RequestLine> requestMessageHeader = 
        MessageHeader.newRequestInstance(
            Method.GET, "/hello.txt", "HTTP/1.1", 
            HeaderField.newInstance(
                "User-Agent", 
                "curl/7.16.3 libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3"),
            HeaderField.newInstance("Host", "www.example.com"),
            HeaderField.newInstance("Accept-Language", "en, mi"));
    
    outputStream.write(requestMessageHeader.toByteArray());
    outputStream.flush();
    
    Reader reader = new InputStreamReader(
        inputStream, Charset.forName("UTF-8"));
    
    MessageHeader<StatusLine> statusMessageHeader = 
        MessageHeader.newStatusInstanceFrom(reader);
    
    if (statusMessageHeader.getStartLine().getStatusCode() != 200) {
        // handle non-OK status message
    }
    
    // process the contents of the message body from inputStream
    
```

```java
    
    // As a server
    
    // inputStream and outputStream are initialized outside this example
    
    Reader reader = new InputStreamReader(
        inputStream, Charset.forName("UTF-8"));
    
    MessageHeader<RequestLine> requestMessageHeader = 
        MessageHeader.newRequestInstanceFrom(reader);
    
    MessageHeader<StatusLine> statusMessageHeader = null;
    
    // check if the request message is valid
    
    // if the request message is not valid, 
    // send an error status message to outputStream
    
    // if the request message is valid, 
    // send an OK status message with payload to outputStream
        
    // in this case, the request message is valid
    statusMessageHeader = MessageHeader.newStatusInstance(
        "HTTP/1.1", 200, "OK", 
        HeaderField.newInstance(
            "Date", "Mon, 27 Jul 2009 12:28:53 GMT"),
        HeaderField.newInstance("Server", "Apache"),
        HeaderField.newInstance(
            "Last-Modified", "Wed, 22 Jul 2009 19:15:56 GMT"),
        HeaderField.newInstance("ETag", "\"34aa387-d-1568eb00\""),
        HeaderField.newInstance("Accept-Ranges", "bytes"),
        HeaderField.newInstance("Content-Length", "51"),
        HeaderField.newInstance("Vary", "Accept-Encoding"),
        HeaderField.newInstance("Content-Type", "text/plain"));
    
    ByteArrayOutputStream out = new ByteArrayOutputStream();
    out.write(statusMessageHeader.toByteArray());
    out.flush();
    
    Writer writer = new OutputStreamWriter(
        out, Charset.forName("UTF-8"));
    writer.write("Hello World! My payload includes a trailing CRLF.");
    writer.write("\r\n");
    writer.flush();
    
    outputStream.write(out.toByteArray());
    outputStream.flush();
    
```

## Requirements

-   Apache Maven&#8482; 3.3.9 or higher (for generating javadocs, automated testing, installing, and building) 
-   Java&#8482; SDK 1.8 or higher

## Generating Javadocs

To generate javadocs, run the following commands:

```bash

    cd httpshuttle
    mvn javadoc:javadoc

```

After running the aforementioned commands, the javadocs can be found in the following path:

```text
    
    target/site/apidocs
    
```

## Automated Testing

To run automated testing, run the following command:

```bash

    mvn test

```

## Installing

To install, run the following command:

```bash

    mvn install

```

To add a dependency on HttpShuttle using Maven, use the following:

```xml

    <dependency>
    	<groupId>httpshuttle</groupId>
    	<artifactId>httpshuttle</artifactId>
    	<version>1.0.0-SNAPSHOT</version>
    </dependency>

```

## Building

To build and package HttpShuttle as a jar file, run the following command:

```bash

    mvn package

```

After running the aforementioned command, the jar file can be found in the following path:

```text
    
    target/httpshuttle-${VERSION}.jar
    
```

`${VERSION}` is replaced by the actual version shown within the name of the jar file.
