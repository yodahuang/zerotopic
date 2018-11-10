## Zero Message

*Zero Message* is a lightweight ROS-like pub-sub tool for Python 3.4+.

- Provides a wrapper around [ZeroMQ](http://zeromq.org) socket.
- Communicate between any Python program using publisher-subscriber protocol

### Installation

```
pip install zero_message
```

### Quick start

Refer to the `/examples`:

```python
# listener.py
import asyncio
import zmq
import zmq.asyncio
from zero_message import EnvelopSocket

port = "5556"
context = zmq.asyncio.Context()
socket = EnvelopSocket.as_subscriber(context, port)

def doSomething(msg):
    print(msg)

subscribe_coroutine = socket.subscribe('test', doSomething)

asyncio.get_event_loop().run_until_complete(subscribe_coroutine())
```

```python
# talker.py
import zmq
import time
from zero_message import EnvelopSocket

port = "5556"
context = zmq.Context()
socket = EnvelopSocket.as_publisher(context, port)

while True:
    socket.publish('test', {
        'data': [1, 2, 3]
    })
    time.sleep(1)
```

### Command Line tools

A `rostopic` like tool is provided.

```
zerotopic echo topic_name
```