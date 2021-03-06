# OpenLambda

OpenLambda is a project dedicated to bringing the Lambda ("serverless") computing framework to the open source world. The purpose of this is primarily to provide a platform for the exploration of research questions that serverless computing prompts (see the publication section).

## Getting Started

These instructions will get you a copy of the project up and running in your environment for development and testing purposes. Because Docker requires root privileges and can consume a lot of disk space in the root directory, it is recommended to use OpenLambda in a virtual environment.

### Installation

Grab and extract the open-lambda source package from github

```
wget https://github.com/open-lambda/open-lambda/archive/v0.1.1.tar.gz
```

Use the startup script to install requirements, build the project using the Makefile.

```
cd open-lambda-0.1.0
./tools/quickstart/startup.sh
make
```

### Starting a lambda cluster locally
Use the start-cluster script to start a local cluster for testing. The cluster will use the Docker registry by default, or the OpenLamdba code registry if passed the optional flag

```
./util/start-cluster.py [--olregistry]
```

Use the setup.py script to push application handler code to the registry and run startup scripts (must be in a directory under the applications directory)

```
./util/setup.py --appdir pychat --appfile lambda_func.py
```

Send RPCs to the URL generated by the setup.py script

```
cat applications/pychat/static/config.json
curl -w "\n" <URL> -d '{"op": "init"}'
curl -w "\n" <URL> -d '{"op": "updates", "ts": "0"}'
curl -w "\n" <URL> -d '{"op": "msg", "msg": "hello"}'
```

## Running the tests

To run the full suite of tests:

```
make test
```

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

## Architecture

Diagram of overall architecture coming soon.

### Load Balancers

All RPC requests to the system are issued directly to a load balancer. There, the load balancing policy selects a worker to handle the request, and proxies it.

Currently, we are using an Nginx instance for load balancing, with simple round-robin scheduling. We hope to soon experiment with more interesting load balancing policies, as the Lambda model prompts many interesting locality questions.

### Registry

The registry is a shared code store for all of the registered handlers for RPCs. Workers pull code from this registry when they do not have the handler locally. This is often the slowest part of executing an RPC, which makes directing requests to a worker which has the handler locally an important consideration.

There are currently two implemented registries for use with OpenLambda, the Docker registry and our native [code registry](https://github.com/open-lambda/code-registry).

### Workers

### Sandboxes



## Built With

* Docker
* go-dockerclient
* Nginx
* gRPC

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Authors

* Scott Hendrickson [phonyphonecall](https://github.com/phonyphonecall)
* Ed Oakes [edoakes](https://github.com/edoakes)
* Tyler Harter [tylerharter](https://github.com/tylerharter)

See also the complete list of [contributors](https://github.com/open-lambda/open-lambda/contributors) who participated in this project.

## License

This project is licensed under the Apache License - see the [LICENSE.md](LICENSE.md) file for details.
