```
$ kubectl apply -f keda-config/jobs/secret.yaml
$ kubectl apply -f keda-config/jobs/keda.yaml
```

+   - Verify that your scaled job is ready by running:

```
$ kubectl get ScaledJob
$ kubectl get TriggerAuthentication
```

+   - You can test your KEDA deployment by running a test script. First you need to set up the Python environment:

```
$ python3 -m venv .
$ source bin/activate
$ pip3 install logger
$ pip3 install azure-servicebus
```

+   - Now you need to create your program that sends messages to the Azure Service Bus queue. You will need to modify connection_string and queue_name:

```
import os
import sys
import time
import yaml
from logger import logger
from azure.servicebus import ServiceBusClient,
ServiceBusMessage
```

```
def send_a_list_of_messages(sender):
    messages = [ServiceBusMessage(f"Message in list {i}")
    for i in range(1000)]
    sender.send_messages(messages)
    logger.info("Sent a list of 1000 messages")
```