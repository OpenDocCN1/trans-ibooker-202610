```
-> span.set_tag('error', random_flag)

-> span.log_kv({'event': 'CheckCinema' , 'value':
showtime_details })
```

You now can simply run this program in your Python console by passing a made-up movie title as the argument, as follows:

```
$ python jaeger_opentracing.py godfather
Initializing Jaeger Tracer with UDP reporter
Using sampler ConstSampler(True)
opentracing.tracer initialized to <jaeger_client.tracer.Tracer object at 0x10eeae410>[app_name=movie_booking]
Reporting span
f99b147b abd58321:5ca566beb40e89b0:931be7dc309045fd:1
movie_booking.CheckCinema
Reporting span
f99b147b abd58321:1ad5d00d2acbd02:931be7dc309045fd:1
movie_booking.CheckShowtime
Reporting span
f99b147b abd58321:f36b9959f34f61dc:931be7dc309045fd:1
movie_booking.BookShow
Reporting span f99b147b abd58321:931be7dc309045fd:0:1
movie_booking.booking
```

You can now access the Jaeger UI and look for movie_booking in the service’s drop-down menu, and you can click the Find Traces button and see the traces appear, as shown in Figure 6-11.

![](bbox=[0.14, 0.63, 0.86, 0.74])

Figure 6-11. Traces for the movie-booking service

You can see the four spans in Figure 6-11, with movie_booking being the root span and the rest being the child spans. Since this is a sample program, for demonstration purposes we randomly generated some errors in the code which are being bubbled up. In production, these errors would