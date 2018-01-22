# Epiphany

An in development network discovery service. Epiphany facilitates the broadcast of a discovery packet to the network so any number of clients running Epiphany can see it.

## Getting Started

Epiphany supports python 3. To install it, simply run `pip install epiphany`.

#### Step 1
Initialize Epiphany using a port dedicated for discovery broadcasts. Here, we'll use `6000`.
```python
from epiphany import Epiphany, Service

epiphany = Epiphany(port=6000)
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

# Register the service with Epiphany
epiphany.register_service(MasterService)
```

#### Step 3
Then register a listener. This will be triggered every time it receives the broadcast packet. (By default, it would be approximately every 5 seconds)
```python
@epiphany.register_listener('test')
def on_discovery_test(protocol, service):
  print('%s: %s:%d' % (service.id, service.ip, service.port))
  print(service.payload)
```

## Authors

* **Trevin Miller** - *Initial work* - [Stumblinbear](https://github.com/Stumblinbear)

See also the list of [contributors](https://github.com/Secret-Web/Epiphany/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details