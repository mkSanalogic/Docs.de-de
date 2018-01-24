# <a name="aspnet-core-response-caching-sample-aspnet-core-2x"></a><span data-ttu-id="9b66d-101">ASP.NET Core-Antwort, die Caching-Beispiel (ASP.NET Core 2.x)</span><span class="sxs-lookup"><span data-stu-id="9b66d-101">ASP.NET Core Response Caching Sample (ASP.NET Core 2.x)</span></span>

<span data-ttu-id="9b66d-102">Dieses Beispiel veranschaulicht die Verwendung von ASP.NET Core [Antwort zwischenspeichern Middleware](xref:performance/caching/middleware) mit ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="9b66d-102">This sample illustrates the usage of ASP.NET Core [Response Caching Middleware](xref:performance/caching/middleware) with ASP.NET Core 2.x.</span></span> <span data-ttu-id="9b66d-103">Das ASP.NET Core 1.x-Beispiel finden Sie unter [ASP.NET Core Antwort Caching-Beispiel (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).</span><span class="sxs-lookup"><span data-stu-id="9b66d-103">For the ASP.NET Core 1.x sample, see [ASP.NET Core Response Caching Sample (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).</span></span>

<span data-ttu-id="9b66d-104">Die Anwendung sendet eine `Hello World!` Nachricht und die aktuelle Zeit zusammen mit einem `Cache-Control` Header so konfigurieren Sie das Verhalten beim Zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="9b66d-104">The application sends a `Hello World!` message and the current time along with a `Cache-Control` header to configure caching behavior.</span></span> <span data-ttu-id="9b66d-105">Sendet die Anwendung außerdem eine `Vary` Header so konfigurieren Sie den Cache in der Antwort nur, wenn dienen der `Accept-Encoding` -Header der nachfolgenden Anforderungen, die aus der ursprünglichen Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="9b66d-105">The application also sends a `Vary` header to configure the cache to serve the response only if the `Accept-Encoding` header of subsequent requests matches that from the original request.</span></span>

<span data-ttu-id="9b66d-106">Beim Ausführen des Beispiels wird eine Antwort aus dem Cache, wenn möglich und gespeicherte für bis zu 10 Sekunden verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="9b66d-106">When running the sample, a response is served from cache when possible and stored for up to 10 seconds.</span></span>