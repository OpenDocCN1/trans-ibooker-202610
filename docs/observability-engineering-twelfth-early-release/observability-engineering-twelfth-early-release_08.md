# 第 7 章. 使用 OpenTelemetry 进行仪器化

## 早期发布读者的说明

使用 Early Release 电子书，你可以在这些标题正式发布之前很久就利用这些技术——作者在写作时提供的原始和未编辑内容，以便你能够提前利用这些技术。

这将是最终书籍的第 7 章。

如果你对我们如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系编辑 vwilson@oreilly.com。

在前两章中，我们描述了结构化事件和 tracing 的原则。事件和 trace 是你可以用来自理解软件应用程序行为的可观测性构建块。你可以通过在应用程序中添加仪器化代码来在每次调用时发出遥测数据，从而生成这些基本构建块。然后你可以将发出的遥测数据路由到后端数据存储，以便稍后分析以了解应用程序健康状况并帮助调试问题。

在本章中，我们将向你展示如何对代码进行仪器化以发出遥测数据。你选择的方案可能取决于你的可观测性后端支持的仪器化方法。供应商通常会创建专有的 APM、指标或 tracing 库来为其特定解决方案生成遥测数据。然而，为了本厂商中立书籍的目的，我们将描述如何实现



instrumentation with open source standards that will work with a wide variety of backend telemetry stores.

This chapter starts by introducing the OpenTelemetry standard and its approach for automatically generating telemetry from applications. Telemetry from automatic instrumentation is a fine start, but the real power of observability comes from custom attributes that add context to help you debug how your intended business logic is actually working. We’ll show you how to use custom instrumentation to augment the out-of-the-box instrumentation included with OpenTelemetry. By the end of the chapter, you’ll have an end-to-end instrumentation strategy to generate useful telemetry data. And in later sections of this book, we’ll show you how to analyze that telemetry data to find the answers you need.

## A Brief Introduction to Instrumentation

It is a well-established practice in the software industry to instrument applications to send data about system state to a central logging or monitoring solution. That data, known as *telemetry*, records what your code did when processing a particular set of requests. Over the years, software developers have defined various overlapping categories of telemetry data, including logs, metrics, and, more recently, traces and profiles.

As discussed in [Chapter 5], arbitrarily wide events are the ideal building blocks for observability. But wide events are not new; they are just a special kind of log that consists of many structured data fields rather than fewer, more ambiguous blobs. And a trace (see [Chapter 6]) comprises multiple wide events with embedded fields to connect disparate events.

However, the approach to application instrumentation has typically been proprietary. Sometimes producing the data requires manually adding code to generate telemetry data within your services; other times, an agent can assist in gathering data automatically. Each distinct monitoring solution (unless it’s `printf()`) has its own custom set of necessary steps to generate and transmit telemetry data in a format appropriate for that product’s backend data store.

This chapter will show you how to jumpstart your observability journey with the flexibility and power of OTel. You can use these lessons to quickly instrument your applications and use that data in any number of observability solutions.

## Instrumentation Using Code-Based Examples

At the time of publication, OTel supports instrumenting code written in many languages (including Go, Python, Java, Ruby, Erlang, PHP, JavaScript, .NET, Rust, C++, and Swift), and also includes a common message standard and collector agent.

To provide practical examples of instrumentation rather than confining ourselves to abstract concepts, we need to utilize one specific language that OTel supports. We use the Go programming language for these examples because it is the language Kubernetes is written in and a lingua franca of modern distributed microservices and cloud native systems. However, you can apply these concepts to any language because OTel offers standard terminology and API design (when possible) in each language it supports.

You shouldn’t need to be a Go programmer to understand these examples. However, we’ll also explain what each of the following code snippets does in detail.

## A CRASH-COURSE IN OPENTELEMETRY CONCEPTS

OpenTelemetry provides the libraries, agents, tooling, and other components designed for the creation and management of telemetry data from your services. OTel brings together APIs for different telemetry data types and manages the distributed context propagation common to all of them. It combines concepts and components from each of its predecessor projects to retain their advantages while still being backward compatible with previously written instrumentation.

To do this, OTel breaks functionality into several components with a distinct set of terminology. Useful concepts to keep in mind with OTel are as follows:

*API*

The specification portion of OTel libraries that allows developers to add instrumentation to their code without concern for the underlying implementation.

*SDK*

The concrete implementation component of OTel that tracks state and batches data for transmission.

*Tracer*

A component within the SDK that is responsible for tracking which span is currently active in your process. It also allows you to access and modify the current span to perform operations like adding attributes, events, or finishing it when the work it tracks is complete.

*Meter*

A component within the SDK that is responsible for tracking which metrics are available to report on in your process. It also allows you to access and modify the current metrics to perform operations like adding values or retrieving those values at periodic intervals.

## Context propagation

An integral part of the SDK that deserializes context about the current inbound request from headers such as W3C `TraceContext` or B3M, tracks what the current request context is within the process, and serializes it to pass downstream to a new service.

## Exporter

A plug-in for the SDK that translates OTel in-memory objects into the appropriate format for delivery to a specific destination.

Destinations may be local (such as a log file or stdout) or may be remote (backends can be open source, such as Jaeger, or proprietary commercial solutions, like Honeycomb or Lightstep). For remote destinations, OTel defaults to using an exporter speaking the wire protocol known as OpenTelemetry Protocol (OTLP), which can be used to send data to the OpenTelemetry Collector.

## Collector

A standalone binary process that can be run as a proxy or sidecar that receives telemetry data (by default in OTLP format), processes it, and tees it to one or more configured destinations.

A deep dive into OTel components is beyond the scope of this book, but you can find out more about its concepts by reading the [OpenTelemetry documentation](OpenTelemetry documentation).

## Start with Automatic Instrumentation

One of the largest challenges of adopting distributed tracing is getting enough useful data to be able to map it to the knowledge you already have about your systems. How can you teach your observability systems about the services, endpoints, and dependencies you have so that you can start gaining insight? While you *could* manually start and end trace spans for each request, there’s a better way to decrease the friction and get insight in hours or days, not months.

For this purpose, OTel includes automatic instrumentation to minimize the time to first value for users. Because OTel’s charter is to ease adoption of the cloud native ecosystem and microservices, it supports the most common frameworks for interactions between services. For example, OTel automatically generates trace spans for incoming and outgoing gRPC, HTTP, and database/cache calls from instrumented services. This will provide you with at least the skeleton of who calls whom in the tangled web of microservices and downstream dependencies.

To provide you with that automatic instrumentation of request properties and timings, the framework needs to call OTel before and after handling each request. Thus, common frameworks often support wrappers, interceptors, or middleware that OTel can hook into in order to automatically read context propagation metadata and create spans for each request. In Go, this configuration is explicit because Go requires explicit type safety and compile-time configuration; however, in languages such as Java and .NET, you can attach a standalone OpenTelemetry Agent at runtime that will infer which frameworks are running and automatically register itself.

To illustrate the wrapper pattern, in Go, the common denominator for Go is the `http.Request/http.Response HandlerFunc` interface. You can use the otelhttp package to wrap an existing HTTP handler function before passing it into the HTTP server’s default request router (known as a mux in Go):

```go
import
"go.opentelemetry.io/contrib/instrumentation/net/http/otelhttp"

mux.Handle("/route",
	otelhttp.NewHandler(otelhttp.WithRouteTag("/route",
	http.HandlerFunc(h)), "handler_span_name"))
```

In contrast, gRPC provides an Interceptor interface that you can provide OTel with to register its instrumentation:

```go
import
"go.opentelemetry.io/contrib/instrumentation/google.golang.org/grpc/otelgrpc"

s := grpc.NewServer(
	grpc.UnaryInterceptor(otelgrpc.UnaryServerInterceptor()),
	grpc.StreamInterceptor(otelgrpc.StreamServerInterceptor()),
)
```

Armed with this automatically generated data, you will be able to find problems such as uncached hot database calls, or downstream dependencies that are slow for a subset of their endpoints, from a subset of your services. But this is only the very beginning of the insight you’ll get.

## Add Custom Instrumentation

Once you have automatic instrumentation, you have a solid foundation for making an investment in custom instrumentation specific to your business logic. You can attach fields and rich values, such as client IDs, shard IDs, errors, and more to the auto-instrumented spans inside your code. These annotations make it easier in the future to understand what’s happening at each layer.

By adding custom spans within your application for particularly expensive, time-consuming steps internal to your process, you can go beyond the



automatically instrumented spans for outbound calls to dependencies and get visibility into all areas of your code. This type of custom instrumentation is what helps you practice observability-driven development, where you create instrumentation alongside new features in your code so that you can verify it operates as you expect in production in real time as it is being released (see Chapter 11). Adding custom instrumentation to your code helps you work proactively to make future problems easier to debug by providing full context—that includes business logic—around a particular code execution path.

## Starting and finishing trace spans

As detailed in Chapter 6, trace spans should cover each individual unit of work performed. Usually, that unit is an individual request passing through one microservice, and it can be instantiated at the HTTP request or RPC layer. However, in certain circumstances, you might want to add additional details to understand where execution time is being spent, record information about subprocessing that is happening, or surface any number of additional details.

OTel’s tracers store information on which span is currently active inside a key of the context.Context (or in thread-locals, as appropriate to the language). When referring to context in OTel, we should disambiguate the term. Context may be referring not only to its logical usage—the circumstances surrounding an event in your service—but also to specific types of context, like a trace’s span context. Thus, to start a new span, we must obtain from OTel an appropriate tracer for our component, and pass it the current context to update:

```go
import "go.opentelemetry.io/otel"

// Within each module, define a private tracer once for easy identification
var tr = otel.Tracer("module_name")

func funcName(ctx context.Context) {
    sp := tr.Start(ctx, "span_name")
    defer sp.End()
    // do work here
}
```

This snippet allows you to create and name a child span of whatever span already existed in the context (for instance, from an HTTP or gRPC context), start the timer running, calculate the span duration, and then finalize the span for transmission to a backend in the `defer` performed at the end of the function call.

## Adding wide fields to an event

As you’ve seen in earlier chapters, observability is best served by batching up data from the execution of each function into one singular wide event per request. That means you can, and should, add in any custom field you find worthwhile for determining the health of your services or the set of users impacted, or understanding the state of your code during its execution.

OTel makes it easy to add custom fields to any gathered telemetry data by transiently buffering and collating any fields added by your instrumentation during the execution of each request. The OTel data model retains open spans until they’re marked finished. Therefore, you can attach numerous distinct fields of metadata to the telemetry that is ultimately written as one single span. At smaller scales, you can handle schema management informally with constants in your code, but at larger scales you’ll want to see Chapter 18 for how to normalize schemas with telemetry pipelines.

Anywhere you have an active span in the process or thread context, you can make a call like `sp := trace.SpanFromContext(ctx)` to get the currently active span from the active context object. In languages with implicit thread-local context, no explicit context argument need be passed.

Once you have the current active span, you can add fields or events to it. In Go, you must explicitly specify the type for maximum performance and type safety:

```go
import "go.opentelemetry.io/otel/attribute"
sp.SetAttributes(attribute.Int("http.code", resp.ResponseCode))
sp.SetAttributes(attribute.String("app.user", username))
```

This snippet sets two key-value pair attributes. The attribute http.code contains an integer value (the HTTP response code), and app.user contains a string including the current username in the execution request.

## Recording process-wide metrics

In general, most metrics such as measures and counters should be recorded as attributes on the trace span to which they pertain. For instance, Value in Cart is probably best associated with a span in which the user’s cart is processed rather than being aggregated and scraped by a metrics system. However, if you absolutely need an exact, unsampled count that is pre-aggregated by specific dimensions, or need a non-request-specific, process-wide value that is periodically scraped, you can update a measure or counter with a collection of tags:

```go
import "go.opentelemetry.io/otel"
import "go.opentelemetry.io/otel/metric"

// similar to instantiating a tracer, you will want a meter per package.
var meter = otel.Meter("example_package")

// predefine our keys so we can reuse them and avoid overhead
var appKey = attribute.Key("app")
var containerKey = attribute.Key("container")

goroutines, _ := metric.Must(meter).NewInt64Measure("num_goroutines",
    metric.WithKeys(appKey, containerKey),
    metric.WithDescription("Amount of goroutines running."),
)

// Then, within a periodic ticker goroutine:
meter.RecordBatch(ctx, []attribute.KeyValue{
    appKey.String(os.Getenv("PROJECT_DOMAIN")),
    containerKey.String(os.Getenv("HOSTNAME"))},
    goroutines.Measurement(int64(runtime.NumGoroutine())),
    // insert other measurements performed on a measure here.
)
```

This periodically records the number of running goroutines and exports the data via OTel.

When following the path of observability, we strongly recommend that you not box yourself into using the rigid and inflexible measures provided by most application-level metrics. We explore that topic further in [Chapter 9](#). For now, a similar method to the preceding one could be used if you find that you absolutely need to record the value of a metric.

## Send Instrumentation Data to a Backend System

By default, OTel assumes you will be sending data to a local sidecar running on a default port; however, you may want to explicitly specify how to route the data in your application start-up code.

After creating telemetry data by using the preceding methods, you’ll want to send it somewhere. OTel supports two primary methods for exporting data from your process to an analysis backend; you can proxy it through the OpenTelemetry Collector (see [Chapter 18](#)) or you can export it directly from your process to the backend.

Exporting directly from your process requires you to import, depend on, and instantiate one or more exporters. *Exporters* are libraries that translate OTel’s in-memory span and metric objects into the appropriate format for various telemetry analysis tools.

Exporters are instantiated once, on program start-up, usually in the main function. Typically, you’ll need to emit telemetry to only one specific backend. However, OTel allows you to arbitrarily instantiate and configure many exporters, allowing your system to emit the same telemetry to more than one telemetry sink at the same time. One possible use case for exporting to multiple telemetry sinks might be to ensure uninterrupted access to your current production observability tool, while using the same telemetry data to test the capabilities of a different observability tool you’re evaluating.

With the ubiquity of OTLP, the most sensible default is to always configure the OTLP gRPC exporter to either your vendor or to an OpenTelemetry Collector (see [Chapter 18](#)) rather than use the vendor’s custom code:

```go
driver := otlgrpc.NewClient(
    otlgrpc.WithTLSCredentials(credentials.NewClientTLSFromCert(nil,
        "")),
    otlgrpc.WithEndpoint("my.backend.com:443"),
)
otExporter, err := otlp.New(ctx, driver)
tp := sdktrace.NewTracerProvider(
    sdktrace.WithSampler(sdktrace.AlwaysSample()),
    sdktrace.WithResource(resource.NewWithAttributes(
        semconv.SchemaURL,
        semconv.ServiceNameKey.String(serviceName))),
    sdktrace.WithBatcher(otExporter))
```

But if you must configure multiple custom exporters for backends that are neither an OpenTelemetry Collector nor support the common OTLP standard, here is an example of instantiating two exporters in the application start-up code:

```go
import (
    x "github.com/my/backend/exporter"
    y "github.com/some/backend/exporter"
    "go.opentelemetry.io/otel"
    sdktrace "go.opentelemetry.io/otel/sdk/trace"
)

func main() {
    exporterX := x.NewExporter(...)
    exporterY := y.NewExporter(...)
    tp, err := sdktrace.NewTracerProvider(
        sdktrace.WithSampler(sdktrace.AlwaysSample()),
        sdktrace.WithSyncer(exporterX),
        sdktrace.WithBatcher(exporterY),
    )
    otel.SetTracerProvider(tp)
}
```

In this example code, we import both *my/backend/exporter* and *some/backend/exporter*, configure them to synchronously or in batches receive trace spans from a tracer provider, and then set the tracer provider as the default tracer provider. This machinery causes all subsequent calls to `otel.Tracer()` to retrieve an appropriately configured tracer.





## 结论

尽管有几种方法可以为应用程序插桩以实现可观测性，但 OTel 是一种开源标准，它使您能够将遥测数据发送到您选择的任意数量的后端数据存储。OTel 正成为一种新的厂商中立方法，确保无论您选择哪种可观测性系统，都能对应用程序进行插桩以发出遥测数据。

本章中基于代码的示例使用 Go 编写，但相同的术语和概念适用于 OTel 当前支持的任何语言。您可以使用相同的步骤来利用自动插桩的强大功能、向事件添加自定义字段、启动和结束追踪跨度，以及实例化导出器。如有必要，您甚至可以向追踪中添加指标。

但这引出了问题：在考虑指标时，为什么要加上“如有必要”这个告诫呢？指标是一种具有内在局限性的数据类型。在接下来的两章中，我们将探讨指标和事件的使用，以进一步阐明这一考量。
