# Testing Periodic Actions

At last we can get back to our beloved Soggy Waffle application. We saw how to test a GenServer whose job is essentially to *hold some state*. Another common use for GenServers is to have a process in your system that performs an action *periodically*. For example, you could have a GenServer that resets a cache every five minutes or a GenServer that looks for changes to system environment variables every few seconds and updates other parts of your system when that happens.

For Soggy Waffle, we need something like what we just described. Soggy Waffle’s purpose is to alert us in case there’s going to be rain in the next few hours. To do that, the application needs to *periodically* call out to the weather API to check if there’s rain in the forecast for a given location. Remember the weather API we worked with in Chapter 1, [Unit Tests](#), and Chapter 2, [Integration and End-to-End Tests](#)? We’ll finally be able to put that to real use.

Performing weather API calls periodically and alerting if there’s rain in the next few hours isn’t enough to make this work properly. The application also needs to avoid alerting us every time the forecast says it’s going to rain in two hours: if we check the API every 30 minutes and the forecast says it’ll rain in two hours, we can’t alert every thirty minutes. We already get alerted by too much stuff in this world. We need to store some data somewhere that’ll help us avoid useless alerts. That sounds like keeping state.

So our use case is clear: perform some periodic action and store some state. Seems like our best tool to solve this is exactly a GenServer—what a plot twist.

We have one more detail to cover. We want Soggy Waffle to alert us through SMS. To send SMSs, we’ll just use a third-party integration