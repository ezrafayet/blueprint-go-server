# Clean go architecture 🛡️

Post-OOP languages are doing great implementing ports and adapters in an hexagonal architecture. There are a lot of resources online describing the core principles of hexagonal architecture, and a lot of ressources on how to implement it in Go. They are great. But because they aim at senior developers, I felt they somehow failed at answering the most basic real-life implementation questions: how the heck do I send emails ? Where do I put my middlewares ? How do I make request to other services ? How do I implement a queue ? Should I really turn everything into an Actor ? Or is it ok to have librairies I use with direct calls ?

This repo aims at answering this question: how to achieve buiding a stable, flexible, scalable, highly available REST API in Go.

I have a few years of experience building servers with NodeJs. I've put a lot of pain and suffering (for good) into building / maintaining / refactoring them. Though I am rather new in Go. This is why, if you come accross this project, you should feel free to share and participate.


## Why choosing Go ❓

According to Go's FAQ, "(Before Go), one had to choose either efficient compilation, efficient execution, or ease of programming; all three were not available in the same mainstream language. [...] Go addressed these issues [...]. It also aimed to be modern, with support for networked and multicore computing".

It's safe to say they nailed it.

Additionaly we can mention the standard library is so good it makes using third party libraries useless in most cases, allowing you to achieve next-to-zero dependencies.

In short, Go is a first class language to pick when building a server.


## Why choosing hexagonal ❓

According to it's author, the ports and adapters architecture exists to inforce isolation of the application's components by design, so they can be developed and tested in isolation from its run-time devices and databases. Easy.

- It helps you focus on the business rules,
- It helps you to build a technology-agnostic application,
- It allows you to run tests in isolation,
- It clarifies who does what (each component have a well defined perimeter).

On the 'bad side' it surely is more technical than other patterns.


## Ports and Adapters architecture 🔌

This architecture is about isolating the core business.

To achieve isolation, everything that is not pure business logic and is considered as an Actor driving the application (Primary Actor) or an Actor driven by the application (Secondary Actor) is just removed from the domain and pushed away on the surounding Framework layer. All communication between the actors and the application is achieved through Ports and Adapters.

If you read about this pattern, chances are you came accross a diagram like this:

![fig1](./README/hexagonal_traditional_layers.png "fig1")

Notice how everything that is not part of the core business is simply pushed outside. Notice how those actors are all equally treated. The core do not adapt to them, they adapt to the core. The core define actions for the Primary Actors to use the application (Primary Ports), and defines actions a Secondary Actor has to implement so it can be used by the application (Secondary Ports).

## Dependency injection 💉

This architecture relies heavily on dependency injection for secondry actors. Meaning each layer receives the objects it can call.

Notice how all dependencies point inward. Which means outside layers depend on the inside layers. Which means a file in an inner circle never import a file from an upper level. If it does, something failed.

... todo

## ☠️ Responsibility for each layer ☠️

...todo

## Zoom on a slice

...todo

![fig3](./README/hexagonal_slice.png "fig3")

...todo

## The folder structure 📁

The structure of folders should reflect the separation between each layer. What I propose here is the result of many hours of thinking and refactoring, but it is not definitive and could benefit some external opinion.

```
root
├── cmd -------------------> contains entry points for the program
|   └── httpserver --------> calls httpserver.Start()
├── internal --------------> private application code
|   ├── core --------------> contains all the business logic (models, services, ports).
|   |   ├── domain
|   |   ├── ports
|   |   └── services
|   ├── infrastructure ----> contains all secondary actors, pproviders, the router and the registry.
|   └── interface ---------> contains the interface layer (repository, handlers, middlewares).
├── pkg -------------------> shared code, library-wrappers...
├── .env ------------------> secrets
└── Makefile
```

Note you will find advanced informations on how to structure your project here (technology agnostic): https://github.com/golang-standards/project-layout

### How it maps to the layers

As mentioned above, each folder maps to a specific layer:
| Folder         | Layer                |
|----------------|----------------------|
| core > domain  | Entities             |
| core > service | Use cases            |
| interface      | Adapters             |
| infrastructure | Frameworks & Drivers |

Note Entities, Use cases and Ports are grouped in the same folder since they all are part of the business logic

## Create an entity

## Create an entity port

## Create a service

## Create a service port

## Create a controller

## Use a registry to  connect ports and adapters together 

## Use a router for http requests

## Create a middleware

## Connect a database

## Connect a new secondary actor

## Unitests 🧪

## How to create a new service

## How to create a new use case ? endpoint + handler

## Pub/Sub Workers

## Queues and workers

## Chron jobs ⏰

## Secrets 🤐

## Docker 🐋

## External ressources

- The original article about hexaagonal architectur from Alistair Cockburn: https://alistair.cockburn.us/hexagonal-architecture/
- The Twelve-Factor App: https://12factor.net/
- Go standards on how to structure a project: https://github.com/golang-standards/project-layout
- A clean implementation in Go by Matías Varela: https://medium.com/@matiasvarela/hexagonal-architecture-in-go-cfd4e436faa3
