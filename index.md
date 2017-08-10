### Welcome to Meeko.
Meeko is a tiny single-node platform as a service that makes it very easy to build systems that consist of multiple components (in this case processes) that communicate with each other. The platform exports various services that the components (called *agents*) can use to exchange data. Each service implements one of the popular communication patters, e.g. publish-subscribe or request-reply, or some other functionality that is useful for the agents.

The platform consists of three basic component:

* [meekod](https://github.com/meeko/meekod) is the core component (process) that contains the component supervisor and communication broker.
* [meeko](https://github.com/meeko/meeko) is a command line management utility that can be used to install new components into the system, then configure and start them.
* [go-meeko](http://godoc.org/github.com/meeko/go-meeko/agent) is the Go client library that can be used by the components to connect to all the communication services. It makes the communication very simple and friendly.

### Demo

```
$ meeko install \\
    git+https://github.com/meeko-contrib/meeko-collector-github \\
    as gh-collector
>>> Creating the agent workspace ... OK
>>> Cloning the agent repository ...  
Cloning into '/var/lib/meeko/workspace/gh-collector/_stage'...
<<< OK 
>>> Reading agent.json ... OK 
>>> Validating agent.json ... OK 
>>> Moving files into place ... OK 
>>> Running the install hook ...  
+ godep go install -v
github.com/meeko-contrib/go-meeko-webhook-receiver/receiver/server
github.com/meeko/go-meeko/meeko/services
github.com/meeko/go-meeko/meeko/services/logging
github.com/cihub/seelog
github.com/tchap/go-patricia/patricia
github.com/meeko/go-meeko/meeko/services/pubsub
github.com/meeko/go-meeko/meeko/services/rpc
github.com/dmotylev/nutrition
github.com/pebbe/zmq3
github.com/meeko/go-meeko/meeko/transports/zmq3/logging
github.com/meeko/go-meeko/meeko/transports/zmq3/loop
github.com/ugorji/go/codec
github.com/meeko/go-meeko/meeko/utils/codecs
github.com/meeko/go-meeko/meeko/transports/zmq3/pubsub
github.com/meeko/go-meeko/meeko/transports/zmq3/rpc
github.com/meeko/go-meeko/agent
github.com/meeko-contrib/go-meeko-webhook-receiver/receiver
github.com/meeko-contrib/meeko-collector-github/handler
github.com/meeko-contrib/meeko-collector-github
<<< OK 
>>> Inserting the agent database record ... OK 

Success 
$ meeko info gh-collector
Alias:          gh-collector
Name:           meeko-collector-github
Version:        0.0.1
Description:    Meeko collector for GitHub events
Repository:     git+https://github.com/meeko-contrib/meeko-collector-github
Variables:
                Name:   LISTEN_ADDRESS
                Usage:  TCP network address to listen on; format HOST:PORT
                Type:   string
                Value:  unset

                Name:   ACCESS_TOKEN
                Usage:  token to be used for hooks authentication
                Type:   string
                Value:  unset

# Listen on localhost because we are behind Nginx.
$ meeko set LISTEN_ADDRESS for gh-collector to localhost:8888

Success
$ meeko set -ask ACCESS_TOKEN for gh-collector
Insert the value of ACCESS_TOKEN: 

Success
$ meeko start -watch gh-collector
>>> Streaming logs for agent gh-collector
[INFO] Logging service initialised
[INFO] PubSub service initialised
[INFO] RPC service initialised
[INFO] Forwarding github.issues
```