---
layout: page
title: development with containerized environments in vscode
description: this document covers the devcontainer extension for vscode, which enables developers to work with containerized development environments
img: 
importance: 1
category: containerization
---
## Introduction

*Devcontainer* is a powerful extension that helps developers create a containerized development environment within *vscode* . It functions similarly to a virtual environment, but provides more flexibility and customization options. With Devcontainer, you can easily automate and reuse your development environments, ensuring that your projects are always running in a consistent and well-defined environment.

Those containerized environments are created in *docker* and they can be accessed through *docker cli*. One of the key advantages of *devcontainer* is its integration with *vscode*, which allows developers to switch between their local and containerized workspaces with just one click. This means that all necessary tools, runtime stacks, and prerequisites are immediately available in the containerized environment, saving developers time and effort. And if the application needs more than one environment, such as two version of *java*, for instance, then, two containerized environments can be created, with specific requirements. And last but not least, every one can use the same configuration file to generate the some containerized environments, drastically reducing setup time.

> It allows using a container as a full-featured development environment which can be used to run an application, to separate tools, libraries, or runtimes needed for working with a codebase, and to aid in continuous integration and testing

This document provides a more in-depth look at the *devcontainer* extension for *vscode*, including examples of its use.

## Under the hood

*Devcontainer* is an extension of *vscode* that needs *docker* to work. All details about system requirements are [here](https://code.visualstudio.com/docs/devcontainers/containers#_getting-started){:target="_blank"}. And [here](https://learn.microsoft.com/en-us/shows/beginners-series-to-dev-containers/?wt.mc_id=devcloud-11496-cxa){:target="_blank"} there is a concise series about how to configure and use *devcontainer*.

Under the hood, the basic idea is that the container encapsulates all resources that are required to run the application (file system, runtime stack, or even tools) and it abstracts them from the local environment, creating a communication channel between local OS and container that allows compiling, running, debugging the source code. This's possible because a *vs code server* runs into the container. As the follow image shows:

![devcontainer architecture](/assets/img/content/dev-container/architecture-containers.png){:.centered}
From code.visualstudio.com/docs.

In addition to the above system requirements, *devcontainer* requires a configuration file called `devcontainer.json`, which is located in the `.devcontainer/` directory. This file contains all metadata and settings needed to configure a development container. The `devcontainer.json` is used to specify how to set up a container for the development purpose when using an image. *Devcontainer* also supports the use of a *dockerfile* and *dockercompose* configuration file, allowing it to act as a multi-container orchestrator in the case of *dockercompose*.

An addition approach allowed by *devcontainer* is acting as a multi-container orchestrator format. For that, it uses *dockercompose* configuration file.

### devcontainer.json

All information about `devcontainer.json` can be found [here](https://containers.dev/implementors/json_reference/){:target="_blank"}. And container images can be found [here](https://github.com/devcontainers/images/tree/main/src){:target="_blank"}. Container images are the simplest way to start with *devcontainer*. They are a *docker* image built with dev container features and with a specific stack. For instance, our example below is a image for a jekyll server environment. Let's analyze a `devcontainer.json` for that purpose.

~~~ json
{
	"name": "Jekyll-Server",
	"image": "mcr.microsoft.com/devcontainers/jekyll:0-buster",

	"otherPortsAttributes": { "onAutoForward" : "ignore" },
	"forwardPorts": [
		4000, // Jekyll server
		35729 // Live reload server
	                ],
	"postCreateCommand": "sh .devcontainer/post-create.sh"
}
~~~

* `name` property is used to display in the UI. Above, it is used *Jekyll-Server* as a meaningful name to indicate the role of the container.
* `image` property is required when using an image from a container registry (dockerhub, github container registry, azure container registry). Otherwise, if using a *dockerfile* then the `build.dockerfile`property is required; or, if using *dockercompose* then the `dockerComposeFile` property is required.
* `otherPortsAttributes` property is used to set options for ports attributes, including the `onAutoForward` property. The default value of `onAutoForward` is `notify`, which means that the port is automatically bound to the host and a notification will appear to indicate this. When the value is `openBrowser` or `silent`, the port is automatically bound to the host as well, but in the case of `openBrowser`, it will automatically open the port in system’s default browser, and in the case of `silent`, the port will be forwarded without any further action being taken. The exception to automatic binding automatically is `ignore` value. In this case, any port will be bound except those specified in the `forwardPorts` property.
* `forwardPorts` property specifies ports that will be forwarded from container to host. As said above, if `onAutoForward` was set to `ignore`, only ports in this list will be forwarded. For instance, if the jekyll server is started on port 3000, it will not be accessible to the host. Any other value sets to `onAutoForward` property, it will automatically bind the port.
* `postCreateCommand` property is related to lifecycle scripts, which means entrypoints at different stages in the container's lifecycle. In this example, `postCreateCommand` stage is the last of three that finalizes container setup when a dev container is created and in our scenario is set to a shell script responsible for install dependencies in the container, as follow:

~~~ shell
#!/bin/sh

# Install the version of Bundler.
if [ -f Gemfile.lock ] && grep "BUNDLED WITH" Gemfile.lock > /dev/null; then
    cat Gemfile.lock | tail -n 2 | grep -C2 "BUNDLED WITH" | tail -n 1 | xargs gem install bundler -v
fi

# If there's a Gemfile, then run `bundle install`
# It's assumed that the Gemfile will install Jekyll too
if [ -f Gemfile ]; then
    bundle install
fi
~~~

`devcontainer.json` properties allow an extensive configuration in the container's environment. An important feature is the configuration of vs code that is running into the container. When the environment is changed in *vscode* from local to container, *vscode* settings change to the settings of *vscode* inside the container.

For solve that, `customizations` property comes into play, which allows changing settings and extensions of *vscode* inside the container. For instance:

~~~ json
"customizations": {
		"vscode": {
			"settings": { 
				"python.defaultInterpreterPath": "/usr/local/bin/python",
				"python.linting.enabled": true,
				"python.linting.pylintEnabled": true,
				"python.formatting.autopep8Path": "/usr/local/py-utils/bin/autopep8",
				"python.formatting.blackPath": "/usr/local/py-utils/bin/black",
				"python.formatting.yapfPath": "/usr/local/py-utils/bin/yapf",
				"python.linting.banditPath": "/usr/local/py-utils/bin/bandit",
				"python.linting.flake8Path": "/usr/local/py-utils/bin/flake8",
				"python.linting.mypyPath": "/usr/local/py-utils/bin/mypy",
				"python.linting.pycodestylePath": "/usr/local/py-utils/bin/pycodestyle",
				"python.linting.pydocstylePath": "/usr/local/py-utils/bin/pydocstyle",
				"python.linting.pylintPath": "/usr/local/py-utils/bin/pylint"
			},
			
			// Add the IDs of extensions you want installed when the container is created.
			"extensions": [
				"ms-python.python",
				"ms-python.vscode-pylance"
			]
		}
	},
~~~

## Dev Container Features

Dev Container Features are additional features that can be added to the container, such as git or any cloud cli, for instance. There is a list of features [here](https://containers.dev/features). `devcontainer.json` accepts a property called `features`, that receives a list of dev container feature ids.

~~~ json
"features": { 
        "ghcr.io/devcontainers/features/github-cli": {}, 
        "ghcr.io/devcontainers/features/git:1": {}
    }
~~~

## Conclusion

*Devcontainer* is a powerful extension that also allows some advanced configuration, which has not been seen here. Next a list of those configurations:

* [Adding environment variables](https://code.visualstudio.com/remote/advancedcontainers/environment-variables);
* [Adding another local file mount](https://code.visualstudio.com/remote/advancedcontainers/add-local-file-mount);
* [Changing or removing the default source code mount](https://code.visualstudio.com/remote/advancedcontainers/change-default-source-mount);
* [Improving container disk performance](https://code.visualstudio.com/remote/advancedcontainers/improve-performance);
* [Adding a non-root user to your dev container](https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user);
* [Avoiding extension reinstalls on container rebuild](https://code.visualstudio.com/remote/advancedcontainers/avoid-extension-reinstalls);
* [Setting the project name for Docker Compose](https://code.visualstudio.com/remote/advancedcontainers/set-docker-compose-project-name);
* [Using Docker or Kubernetes from inside a container](https://code.visualstudio.com/remote/advancedcontainers/use-docker-kubernetes);
* [Connecting to multiple containers at once](https://code.visualstudio.com/remote/advancedcontainers/connect-multiple-containers);
* [Developing inside a container on a remote Docker Machine or SSH host](https://code.visualstudio.com/remote/advancedcontainers/develop-remote-host);
* [Reducing Dockerfile build warnings](https://code.visualstudio.com/remote/advancedcontainers/reduce-docker-warnings);

## References

[containers.dev](https://containers.dev){:target="_blank"}.

[containers.dev: json_reference](https://containers.dev/implementors/json_reference/){:target="_blank"}.

[containers.dev: features](https://containers.dev/features)

[github.com: devcontainers](https://github.com/devcontainers){:target="_blank"}.

[github.com: devcontainers images](https://github.com/devcontainers/images/tree/main/src){:target="_blank"}.

[code.visualstudio.com: containers](https://code.visualstudio.com/docs/devcontainers/containers){:target="_blank"}.