# Epyphany

An in development network discovery service. Epyphany facilitates the broadcast of a discovery packet to the network so any number of clients running Epyphany can see it.

## Getting Started

Epyphany supports python 3. To install it, simply run `pip install epyphany`.

#### Step 1
Initialize Epyphany using a port dedicated for discovery broadcasts. Here, we'll use `6000`.
```python
from epyphany import Epyphany, Service

epyphany = Epyphany(port=6000)
```

#### Step 2
Next, register a service. A service is generally hosted on a port that a client whom receives the broadcast can connect to. The port should not be the same as the discovery port. e.x. `6001`
```python
class TestService(Service):
  def __init__(self):
    # ('test', 6001) = Service ID, Service port
    #   Service ID should be unique
    #   Service Port is the port that the received client should attempt to connect to
    super(TestService, self).__init__('test', 6001)

  def start_service(self):
    # Start up the socket for clients to connect to using "Service Port"
    pass

  def create_payload(self):
    # Any additional data the client may need to know
    return {'foo': 'bar'}

# Register the service with Epyphany
epyphany.register_service(MasterService)
```

#### Step 3
Then register a listener. This will be triggered every time it receives the broadcast packet. (By default, it would be approximately every 5 seconds)
```python
@epyphany.register_listener('test')
def on_discovery_test(protocol, service):
  print('%s: %s:%d' % (service.id, service.ip, service.port))
  print(service.payload)
```

## Authors

* **Trevin Miller** - *Initial work* - [Stumblinbear](https://github.com/Stumblinbear)
* **Bill Miller** - *Some creative inspiration and lots of frustration* - [Secretweb-com](https://github.com/Secretweb-com)

See also the list of [contributors](https://github.com/Secret-Web/Epyphany/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
