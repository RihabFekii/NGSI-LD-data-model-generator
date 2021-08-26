# 'Creating an NGSI-LD data model from scratch' [<img src="https://img.shields.io/badge/NGSI-LD-d6604d.svg" width="90" />] 

This tutorial introduces basics of common Linked Data concepts and Data Models for **NGSI-LD** developers. The aim is to
design and create a simple interoperable Smart Water Quality assesment solution from scratch and explain how to apply these
concepts to your own smart solutions.

The tutorial is mainly concerned with online and command-line tooling.


# Understanding `@context`

Creating an interoperable system of readable links for computers requires the use of a well defined data format
([JSON-LD](http://json-ld.org/)) and assignation of unique IDs
([URLs or URNs](https://stackoverflow.com/questions/4913343/what-is-the-difference-between-uri-url-and-urn)) for both
data entities and the relationships between entities so that semantic meaning can be programmatically retrieved from the
data itself. Furthermore the use and creation of these unique IDs should, as far as possible, be passed around so as not
get in the way of the processing the data objects themselves.

An attempt to solve this interoperability problem has been made within the JSON domain, and this has been done by adding
an `@context` element to existing JSON data structures. This has led to the creation of the **JSON-LD** standard.

# Prerequisites

## Swagger

The OpenAPI Specification (commonly known as Swagger) is an API description format for REST APIs. A Swaggger spec allows
you to describe an entire API (such as NGSI-LD itself) however in this tutorial we shall be concentrating on using
Swagger to define data models.

API specifications can be written in YAML or JSON. The format is easy to learn and readable to both humans and machines.
The complete OpenAPI Specification can be found on GitHub:
[OpenAPI 3.0 Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md). This is
important since we will need a well defined structure to be able to generate `@context` files.

## Docker

To keep things interoperable across different environments the file generator be run using
[Docker](https://www.docker.com). **Docker** is a container technology which allows to different components isolated
into their respective environments.

-   To install Docker on Windows follow the instructions [here](https://docs.docker.com/docker-for-windows/)
-   To install Docker on Mac follow the instructions [here](https://docs.docker.com/docker-for-mac/)
-   To install Docker on Linux follow the instructions [here](https://docs.docker.com/install/)


# How to create from scratch an NGSI-LD data model

In this tutorial I will create a data model for water quality assessment. I am implementing a solution which predicts the potability of water based on water metrics which are the following: "ph", "Hardness", "Solids", "Chloramines", "Sulfate", "Conductivity",  "Organic_carbon", "Trihalomethane", "Turbidity",  "Potability".

To create a data model you need to go through the following steps: 
1. Creating a baseline data model using Swagger
2. Validating the data model
3. Generating the NGSI-LD @context file
4. Creating entities in the Context Broker
5. Writing the API to get notifications from the Context Broker

## Creating a baseline data model using Swagger:

To create the baseline data model it is recommended to use the (Swagger online editor([https://editor.swagger.io/])), once you have it save it in a <basemodel_file>.yaml 
PS: the models are defined within the Smart Data Models domain.
This is a template for a basemodel

[<img src="https://cdn-images-1.medium.com/max/1600/1*obZU_t1lfSiXUR4-POtOLQ.png" />]

Once you have created your base model, you can export it to a Yaml file by clicking on File>Save as Yaml 


## Validating your Swagger data model

It is necessary to check that the model is valid before processing, this can be done by viewing it online or by using a simple validator tool.
To validate you data model you need to: 
Clone this project: https://github.com/FIWARE/tutorials.Understanding-At-Context/tree/NGSI-LD
We need to activate the NGSI-LD generator tool by executing this command in the terminal: 

```console
./services create
```

In the terminal execute this command to validate your data model: 

```console
./services validate [file.yaml]
```
In case of success you get this response in the terminal: 
```console
Yay! the API is valid
```

## Generating the NGSI-LD @context file

Every working linked data system relies on @context files to supply the relevant information about the data. Creating such files by hand is a tedious and error-prone procedure, so it makes sense to automate the process by executing this command in the terminal: 

```console
./services ngsi [file]
```
By opening the generated file you can find a structure like this.

[<img src="https://cdn-images-1.medium.com/max/1600/1*QgFjDI8--s4s2CG4_OByIQ.png" />]

The resultant @context file is a valid JSON-LD Context file usable by NGSI-LD applications. The file is structured into three parts:
A list of standard terms and abbreviations - this avoids the necessity of repeating URIs and reduced the overall size of the file
A series of defined entity types (e.g. Chloramines). These usually start with a capital letter.
A list of attributes and enumerations (not shown in this example)

## Generating documentation of your data model: 

The @context syntax is designed to be readable by machines. Obviously developers need human readable documentation too.
Basic documentation about NGSI-LD entities can be generated from a Swagger data model as follows: 

```console
./services markdown [file]
```

# License

[MIT](LICENSE) © 2020 FIWARE Foundation e.V.
