indicate problems that are occurring in the environment (e.g., Redis timing out due to an I/O issue).

We can also instrument our code using OpenTelemetry, since OpenTracing is now being phased out. First we would need to install the OpenTelemetry API and SDK:

```
pip install opentelemetry-api
pip install opentelemetry-sdk
```

To see how OpenTelemetry works, take a look at the following Python code:

```
from opentelemetry import trace
from opentelemetry.exporter import jaeger
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchExportSpanProcessor

trace.set_tracer_provider(TracerProvider())

jaeger_exporter = jaeger.JaegerSpanExporter(
    service_name="my-helloworld-service",
    agent_host_name="localhost",
    agent_port=6831,
)

trace.get_tracer_provider().add_span_processor(
    BatchExportSpanProcessor(jaeger_exporter)
)

tracer = trace.get_tracer(__name__)

with tracer.start_as_current_span("foo"):
    with tracer.start_as_current_span("bar"):
        with tracer.start_as_current_span("tango"):
            print("Hello world from Opentelemetry! Happy Tracing")
```

The preceding code is a simple “hello world”-style Python script that uses OpenTelemetry. Note how we are using the Jaeger exporter to tell us the location of the Jaeger agent.