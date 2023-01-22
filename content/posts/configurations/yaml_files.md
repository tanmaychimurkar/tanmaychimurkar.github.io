---
title: "Configuration files"
date: 2022-11-17T15:17:29
description: "This post goes over what `configuration` files are and where are they useful"
tags: ["yaml", "json", "ini", "configurations"]
categories: ["configurations", "file types"]
author: "Tanmay"
showToc: true
TocOpen: true
weight: 2
cover:
    image: "https://cdn.lynda.com/course/500545/500545-636159283603072249-16x9.jpg"
    alt: Configuration File
    caption: "A configuration file being used by a Machine to do what the configuration says!"
ShowBreadCrumbs: true
---

## What is a `configuration` file⁉️ 

A `configuration` file is a text file that contains various `settings` and `options` that control the behavior of a software program or system. The file is usually written in a specific format, such as `JSON`, `YAML`, or `INI`, and is read by the program or system at startup. The settings in the configuration file can be modified to change the behavior of the program or system without having to change the code.

A `configuration` file can include options such as the location of other files or resources, user preferences, settings for different environments (e.g. development, production), and various other parameters that control how the program or system behaves. Configuration files are often used to configure servers, network devices, and other types of systems.

For example, a web server might have a `configuration` file that specifies the IP address and port it should listen on, the document root for the web server, and other settings related to security, logging, and performance.

By using configuration files, administrators and developers can easily configure and customize the behavior of a program or system without having to modify the code or recompile the program.

## What is the `JSON` configuration file❓

A `JSON` (JavaScript Object Notation) configuration file is a file format that stores simple data structures and objects in a human-readable, easy-to-access manner. `JSON` files are often used to store configuration settings and data for software applications, and can be easily read and edited by both humans and machines. They are typically saved with a .`JSON` file extension and use a syntax similar to that of JavaScript objects. `JSON` files can be easily parsed and processed by many programming languages, making them a popular choice for configuration files.

Example of a `JSON` file might look like this:

```json
{
    "key1": "value1",
    "key2": "value2",
    "key3": {
        "subkey1": "subvalue1",
        "subkey2": "subvalue2"
    },
    "key4": [
        "arrayvalue1",
        "arrayvalue2",
        "arrayvalue3"
    ]
}
```

### Things to note about a `JSON` file

`JSON` files consist of key-value pairs, similar to a JavaScript object.

The keys are strings, enclosed in double quotes, and the values can be either 'strings', 'numbers', 'objects' (enclosed in curly braces), 'arrays' (enclosed in square brackets), or special values 'true', 'false', and 'null'.

String values must also be enclosed in `double quotes` (`"`).

## What is the `YAML` configuration file❓

`YAML` (short for "YAML Ain't Markup Language") is a human-readable data serialization format that is often used for configuration files, data exchange, and data structuring. It is designed to be easy to read and write, and is often used as an alternative to `XML` or `JSON`.

A `YAML` file is a text file that contains data structured in a specific way. It uses indentation and whitespace to indicate the structure of the data, making it easy to read and understand. The basic building blocks of a `YAML` file are key-value pairs, which are separated by a colon. Lists and arrays are also supported, and can be represented using a simple syntax.

Example of a simple `YAML` file might look like this:

```yaml
name: John Doe
age: 35
address:
    street: 123 Main St
    city: Anytown
    state: CA
    zip: 12345
```
`YAML` files are often used for configuration files because they are easy to read and understand, and are less verbose than other formats such as XML or JSON. They are also used in various tools and frameworks such as Ansible, Kubernetes, and Docker Compose, where they are used to define the configuration of the system.

It's important to note that YAML is case-sensitive and indentation is important, so if you don't use proper indentation or typo the key names it will cause errors. 

### Things to note about a `YAML` file

In a `YAML` file, indentation is used to indicate the hierarchical structure of the data. Each level of indentation represents a new level in the data hierarchy.

For example, in the above `YAML` file, the 'name', 'age', and 'address' keys are at the same level, and are considered to be siblings. The 'street', 'city', 'state', and 'zip' keys are indented under the address key, indicating that they are child elements of the address element. 

It is important to use consistent indentation throughout the `YAML` file. Typically, two spaces are used for each level of indentation, but it can vary. However, it is important to use the same number of spaces throughout the file, as `YAML` is sensitive to indentation. An error in indentation can cause the `YAML` file to be parsed incorrectly, resulting in errors when the file is read by the software that uses it.

Also, `YAML` files use the dash - to indicate the start of a list, for example:

```yaml
fruits:
    - apple
    - banana
    - orange
```
In this example, fruits is a key and the indented items are the list of values for the key.

## What is the `INI` configuration file❓

An `INI` (Initialization) file is a configuration file used by some software applications to store settings and options. It is a simple text file that contains key-value pairs organized into sections. The format of an `INI` file is similar to that of a Windows `INI` file, although it is not limited to Windows.

Here is an example of the structure of an `INI` file:

```yaml
[section1]
key1=value1
key2=value2

[section2]
key3=value3
key4=value4
```

### Things to note about the `INI` file

`INI` files are composed of sections, which are enclosed in square brackets `[]`.

Each section contains one or more `key-value` pairs, separated by an equal sign `=`. The key is a unique string that identifies the value, and the value is an arbitrary string that represents the data.

Comments can also be added by starting the line with a semi-colon `;` or a hash `#`.

`INI` files are often used to store application settings and options, such as user preferences or connection settings.

Unlike `JSON` or `XML`, `INI` files are not formal, standard format and the syntax may vary depending on the application which uses it. They are often used for simple configuration settings where a more complex format is not required.

`NOTE`: It's important to note that the INI file format does not have a formal specification and thus the syntax may vary depending on the application that uses it. However, most implementations follow the above structure and this is a common format for INI files.

## Congratulations🙌🎉🥳🙌🎉🥳

We saw in this post the common types of `configuration files` that are used to create a set of settings that a particular user or a software wants to be executed as defined in the configuration file. 

Now you are a pro at creating your own `configuration` file, and can use as many configuration files as you want in your software!! Until next time 😎😎😎