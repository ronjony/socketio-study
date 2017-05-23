##spray-client

spray-client provides high-level HTTP client functionality by adding another logic layer on top of the relatively basic spray-can HTTP Client APIs. It doesn’t yet provide all the features that we’d like to include eventually, but it should already be of some utility for many applications.

Currently it allows you to wrap any one of the three spray-can client-side API levels with a pipelining logic, which provides for:

Convenient request building

Authentication

Compression / Decompression

Marshalling / Unmarshalling from and to your custom types

Currently, HTTP streaming (i.e. chunked transfer encoding) is not yet supported on the spray-clientlevel (even though the underlying spray-can HTTP Client APIs do support it (the host- and request-level APIs only for responses)), i.e. you cannot send chunked requests and the response-chunk-aggregation-limit config setting for the underlying transport must be non-zero).

Dependencies

Apart from the Scala library (see Current Versions chapter) spray-client depends on

spray-can

spray-http

spray-httpx

spray-util

akka-actor 2.1.x (with ‘provided’ scope, i.e. you need to pull it in yourself)

Installation

The Maven Repository chapter contains all the info about how to pull spray-client into your classpath.

Usage

The simplest of all use cases is this:

import spray.http._ import spray.client.pipelining._ implicit val system = ActorSystem() import system.dispatcher // execution context for futures val pipeline: HttpRequest => Future[HttpResponse] = sendReceive val response: Future[HttpResponse] = pipeline(Get("http://spray.io/"))

The central element of a spray-client pipeline is sendReceive, which produces a function HttpRequest => Future[HttpResponse] (this function type is also aliased to SendReceive). When called without parameters sendReceive will automatically use the IO(Http) extension of an implicitly available ActorSystem to access the spray-can Request-level API. All requests must therefore either carry an absolute URI or an explicit Host header.

In order to wrap pipelining around spray-can‘s Host-level API you need to tell sendReceive which host connector to use:

import akka.io.IO import akka.pattern.ask import spray.can.Http import spray.http._ import spray.client.pipelining._ implicit val system = ActorSystem() import system.dispatcher // execution context for futures val pipeline: Future[SendReceive] = for ( Http.HostConnectorInfo(connector, _) <- IO(Http) ? Http.HostConnectorSetup("www.spray.io", port = 80) ) yield sendReceive(connector) val request = Get("/") val response: Future[HttpResponse] = pipeline.flatMap(_(request))

You can then fire requests with relative URIs and without Host header into the pipeline.

A pipeline of type HttpRequest => Future[HttpResponse] is nice start but leaves the creation of requests and interpretation of responses completely to you. Many times you actually want to send and/or receive custom objects that need to be serialized to HTTP requests or deserialized from HTTP responses. Check out this snippet for an example of what spray-client pipelining can do for you in that regard:

import spray.http._ import spray.json.DefaultJsonProtocol import spray.httpx.encoding.{Gzip, Deflate} import spray.httpx.SprayJsonSupport._ import spray.client.pipelining._ case class Order(id: Int) case class OrderConfirmation(id: Int) object MyJsonProtocol extends DefaultJsonProtocol { implicit val orderFormat = jsonFormat1(Order) implicit val orderConfirmationFormat = jsonFormat1(OrderConfirmation) } import MyJsonProtocol._ implicit val system = ActorSystem() import system.dispatcher // execution context for futures val pipeline: HttpRequest => Future[OrderConfirmation] = ( addHeader("X-My-Special-Header", "fancy-value") ~> addCredentials(BasicHttpCredentials("bob", "secret")) ~> encode(Gzip) ~> sendReceive ~> decode(Deflate) ~> unmarshal[OrderConfirmation] ) val response: Future[OrderConfirmation] = pipeline(Post("http://example.com/orders", Order(42)))

This defines a more complex pipeline that takes an HttpRequest, adds headers and compresses its entity before dispatching it to the target server (the sendReceive element of the pipeline). The response coming back is then decompressed and its entity unmarshalled.

When you import spray.client.pipelining._ you not only get easy access to sendReceive but also all elements of the spray-httpx Request Building and Response Transformation traits. Therefore you can easily create requests via something like Post("/orders", Order(42)), which is not only shorter but also provides for automatic marshalling of custom types.

Example

The /examples/spray-client/ directory of the spray repository contains an example project for spray-client.

simple-spray-client

This example shows off how to use spray-client by querying Google’s Elevation API to retrieve the elevation of Mt. Everest.

Follow these steps to run it on your machine:

Clone the spray repository:

git clone git://github.com/spray/spray.git

Change into the base directory:

cd spray

Run SBT:

sbt "project simple-spray-client" run

(If this doesn’t work for you your SBT runner cannot deal with grouped arguments. In this case you’ll have to run the commands project simple-spray-client and run sequentially “inside” of SBT.)

Next Previous

 
