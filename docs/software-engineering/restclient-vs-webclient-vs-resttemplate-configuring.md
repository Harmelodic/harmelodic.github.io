# RestClient vs. WebClient vs. RestTemplate (Configuring)

There are 3 main HTTP clients available from the Spring Framework: RestClient, WebClient and RestTemplate. This blog
posts aims to cover a comparison of the three from a perspective of a developer who wants to use and configure them.

## Overview

| Header          | RestClient                        | WebClient                        | RestTemplate      |
|-----------------|-----------------------------------|----------------------------------|-------------------|
| Available since | Spring 6.1                        | Spring 5.0                       | Spring 3.0        |
| Ecosystem       | Web / Synchronous                 | Reactive / non-blocking          | Web / Synchronous |
| My perspective? | Recommended for most applications | Useful for reactive applications | Legacy            |

- RestClient
  Javadoc: https://javadoc.io/doc/org.springframework/spring-web/latest/org/springframework/web/client/RestClient.html
- WebClient
  Javadoc: https://javadoc.io/doc/org.springframework/spring-webflux/latest/org/springframework/web/reactive/function/client/WebClient.html
- RestTemplate
  Javadoc: https://javadoc.io/doc/org.springframework/spring-web/latest/org/springframework/web/client/RestTemplate.html

Unless otherwise configured, when used in a Spring Boot context, these clients will automatically encode and decode Java
classes into and from JSON structures (using Jackson).

## Getting a client with Dependency Injection

You could instantiate a client inside your class, but it's much more likely (and better for testing) to perform
dependency injection to get a client (Spring Boot starters will even autoconfigure a Spring Bean for you):

```java
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestClient;

@Component
class ExampleClient {
    private final RestClient restClient;

    ExampleClient(RestClient.Builder restClientBuilder, String baseUrl) {
        this.restClient = restClientBuilder
                .baseUrl(baseUrl) // e.g. https://api.example.com
                .build();
    }
}
```

```java
import org.springframework.stereotype.Component;
import org.springframework.web.reactive.function.client.WebClient;

@Component
class ExampleClient {
    private final WebClient webClient;

    ExampleClient(WebClient.Builder webClientBuilder, String baseUrl) {
        this.webClient = webClientBuilder
                .baseUrl(baseUrl) // e.g. https://api.example.com
                .build();
    }
}
```

```java
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestTemplate;

@Component
class ExampleClient {
    private final RestTemplate restTemplate;
    private final String HOST; // e.g. https://api.example.com

    ExampleClient(RestTemplate restTemplate, String host) {
        this.restTemplate = restTemplate;
        this.HOST = host;
    }
}
```

## Calling to GET a resource

If you're writing something quick & dirty and not error handling response codes or exceptions, handling data is trivial
with all three clients:

```java
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestClient;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

@Component
class ExampleClient {
    private final RestClient restClient;
    private final WebClient webClient;
    private final RestTemplate restTemplate;
    private final String REST_TEMPLATE_HOST;

    Thing fetchThingWithRestClient(String thingId) {
        return restClient.get()
                .uri(uriBuilder -> uriBuilder
                        .path("/thing/{id}")
                        .build(thingId))
                .retrieve()
                .body(Thing.class);
    }

    // Mono<> is a reactive publisher that could contain 0 or 1 thing,
    // kind of like Optional<> but reactive. 
    Mono<Thing> fetchThingWithWebClient(String thingId) {
        return webClient.get()
                .uri(uriBuilder -> uriBuilder
                        .path("/thing/{id}")
                        .build(thingId))
                .retrieve()
                .bodyToMono(Thing.class);
    }

    Thing fetchThingWithRestTemplate(String thingId) {
        // getForObject() is a handy shorthand method for very simple HTTP calls
        // exchange().is more widely used for it's better flexibility
        return restTemplate.getForObject(
                UriComponentsBuilder.fromUri(URI.create(restTemplateBase))
                        .path("/thing/{id}")
                        .build(thingId)
                        .toString(),
                Thing.class
        );
    }

    record Thing(String id, String name) {
    }
}
```

## Calling to GET a list of resources, with a query parameter

If you're writing something quick & dirty and not handling error response codes, but wanting to do simple things like:

- Deal with a list of objects rather than a single object
- Add a query parameter to your call.

then you'll find RestClient & WebClient easy to modify, but RestTemplate requires a change in method.

```java
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestClient;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@Component
class ExampleClient {
    private final RestClient restClient;
    private final WebClient webClient;
    private final RestTemplate restTemplate;
    private final String REST_TEMPLATE_HOST;

    List<Thing> fetchThingWithRestClient(String thingId) {
        return restClient.get()
                .uri(uriBuilder -> uriBuilder
                        .path("/things")
                        .queryParam("nameBeginsWith", "a")
                        .build())
                .retrieve()
                .body(new ParameterizedTypeReference<>() {
                });
    }

    // Flux<> is a reactive publisher that could contain N items,
    // kind of like List<> but reactive.
    Flux<Thing> fetchThingWithWebClient(String beginWith) {
        return webClient.get()
                .uri(uriBuilder -> uriBuilder
                        .path("/things")
                        .queryParam("nameBeginsWith", "a")
                        .build())
                .retrieve()
                .bodyToFlux(Thing.class);
    }

    Thing fetchThingWithRestTemplate(String thingId) {
        // Note how changing we need to move to the more flexible exchange() method
        return restTemplate.exchange(
                RequestEntity.get(UriComponentsBuilder
                                .fromUri(URI.create(restTemplateBase))
                                .path("/things")
                                .queryParam("nameBeginsWith", "a")
                                .encode()
                                .toUriString())
                        .build(),
                new ParameterizedTypeReference<List<Thing>>() {
                }
        ).getBody();
    }

    record Thing(String id, String name) {
    }
}
```

## Calling to POST/PUT/etc. some data

If you're wanting to send data, RestClient and WebClient is again easy to modify, with RestTemplate it's a little more
involved, but provided you're used to the `exchange()` method, not too complicated:

```java
import org.springframework.http.RequestEntity;
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestClient;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

@Component
class ExampleClient {
    private final RestClient restClient;
    private final WebClient webClient;
    private final RestTemplate restTemplate;
    private final String REST_TEMPLATE_HOST;

    Thing sendThingWithRestClient(Thing thing) {
        return restClient.post()
                .uri("/thing")
                .body(thing) // Sending in the request body
                .retrieve()
                .body(Thing.class); // Assuming a Thing is returned in the response body
    }

    // Mono<> is a reactive publisher that could contain 0 or 1 thing,
    // kind of like Optional<> but reactive.
    Mono<Thing> sendThingWithWebClient(Thing thing, Mono<Thing> monoThing) {
        // Either provide a value as the request body:
        return webClient.post()
                .uri("/thing")
                .body(BodyInserters.fromValue(thing))
                .retrieve()
                .bodyToMono(Thing.class);

        // or provide a Mono as the request body: 
        return webClient.post()
                .uri("/thing")
                .body(monoThing, Thing.class)
                .retrieve()
                .bodyToMono(Thing.class);
    }

    Thing sendThingWithRestTemplate(Thing thing) {
        // Continuing usage with the more flexible exchange() method
        return restTemplate.exchange(
                RequestEntity.post(UriComponentsBuilder
                                .fromUri(URI.create(restTemplateBase))
                                .path("/thing")
                                .toUriString())
                        .body(thing),
                Thing.class
        ).getBody();
    }

    record Thing(String id, String name) {
    }
}
```

## Handling HTTP responses

Sometimes, you need to look at the HTTP response. This could be to handle different HTTP status codes in different ways
or to manipulate the response in some way. Both RestTemplate and RestClient can return the `ResponseEntity` class (since
the latter is built using the former), whereas WebClient uses the reactive `ClientResponse` class to allow you to work
with HTTP responses whilst staying in the reactive ecosystem:

```java
import org.springframework.http.RequestEntity;
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestClient;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

@Component
class ExampleClient {
    private final RestClient restClient;
    private final WebClient webClient;
    private final RestTemplate restTemplate;
    private final String REST_TEMPLATE_HOST;

    Thing sendThingWithRestClient(Thing thing) {
        return restClient.post()
                .uri("/thing")
                .body(thing) // Sending in the request body
                .retrieve()
                .body(Thing.class); // Assuming a Thing is returned in the response body
    }

    // Mono<> is a reactive publisher that could contain 0 or 1 thing,
    // kind of like Optional<> but reactive.
    Mono<Thing> sendThingWithWebClient(Thing thing, Mono<Thing> monoThing) {
        // Either provide a value as the request body:
        return webClient.post()
                .uri("/thing")
                .body(BodyInserters.fromValue(thing))
                .retrieve()
                .bodyToMono(Thing.class);

        // or provide a Mono as the request body: 
        return webClient.post()
                .uri("/thing")
                .body(monoThing, Thing.class)
                .retrieve()
                .bodyToMono(Thing.class);
    }

    Thing sendThingWithRestTemplate(Thing thing) {
        // Continuing usage with the more flexible exchange() method
        return restTemplate.exchange(
                RequestEntity.post(UriComponentsBuilder
                                .fromUri(URI.create(restTemplateBase))
                                .path("/thing")
                                .toUriString())
                        .body(thing),
                Thing.class
        ).getBody();
    }

    record Thing(String id, String name) {
    }
}
```

## Exception handling

Assuming you're writing more critical production code, you'll need to be handling response codes and exceptions in order
to produce safer code and less buggy features. Both RestTemplate and RestClient can produce the exceptions:

- `HttpClientErrorException` for 4xx response codes.
- `HttpServerErrorException` for 5xx response codes.
- `ResourceAccessException` for network or other I/O exceptions (including timeouts)

## Configuring timeouts

## Configuring buffer sizes

## Configuring / Customising all clients in an application

## Conclusion

I find RestTemplate always a bit confusing to discover and figure out how I _should_ be writing my code to
perform a HTTP call, given there are handy shorthand methods available, but also the common and powerful `exchange()`
method. The problem is that learning how to write readable, maintainable, flexible `exchange()` methods takes a while to
learn and become familiar with (figuring out what classes are involved, what they mean, and what helpful builders are
available in order to configure what you want).

As such, I find the new fluent APIs of RestClient and WebClient a welcome addition. It still takes a hot minute to
figure out the builders / lambdas on how to configure it, but since there are less varying methods and options, it is
much quicker to learn how things _should_ be written. I also find that the final result is much more readable and
intuitive.

WebClient however, requires the introduction of reactive programming into an application, which isn't required (you
could always just call `block()` on the `Mono<>` or `Flux<>` publishers) but then if you're not going to rewrite your
application code to be completely reactive, then there is little point in using WebClient over RestClient.

As for whether to do reactive programming or not... I'll leave that for another post, but personally, I've found
multiple reasons to not use it, and little reason to do so - and the reasons why I may want to are probably going to be
squashed by Java's upcoming Virtual Threads (Project Loom) feature.
