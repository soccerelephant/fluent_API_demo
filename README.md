# fluent_API_demo
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.fluent.Request;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;

import java.io.BufferedReader;
import java.io.InputStreamReader;

import static org.apache.http.HttpHeaders.USER_AGENT;


public class WebClient {

    public String fetchWebContent(String url) throws Exception {

        HttpClient client = HttpClientBuilder.create().build();
        HttpGet request = new HttpGet(url);

        request.addHeader("User-Agent", USER_AGENT);
        HttpResponse response = client.execute(request);

        BufferedReader rd = new BufferedReader(
                new InputStreamReader(response.getEntity().getContent()));

        StringBuffer result = new StringBuffer();
        String line = "";
        while ((line = rd.readLine()) != null) {
            result.append(line);
        }

        return result.toString();
    }

}

import org.apache.http.client.fluent.Request;


public class WebFluentClient {

    public String fetchWebContent(String url) throws Exception {

        return Request.Get(url).execute().returnContent().asString();
    }
}


import org.junit.Test;

import static org.junit.Assert.assertTrue;


public class WebClientTest {

    @Test
    public void testFetchWebContent() throws Exception {

        String url = "https://www.daugherty.com/";

        WebClient webtClient = new WebClient();

        String content = webtClient.fetchWebContent(url);

        assertTrue(content.contains("Daugherty"));
    }
}



import org.junit.Test;


import static org.junit.Assert.assertTrue;


public class WebFluentClientTest {

    @Test
    public void testFetchWebContent() throws Exception {

        String url = "https://www.daugherty.com/";

        WebFluentClient webFluentClient = new WebFluentClient();

        String content = webFluentClient.fetchWebContent(url);

        assertTrue(content.contains("Daugherty"));
    }
}

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>FluentAPIDemo</groupId>
    <artifactId>FluentAPIDemo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>

        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.3</version>
        </dependency>

        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>fluent-hc</artifactId>
            <version>4.5.3</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.8.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>


</project>
