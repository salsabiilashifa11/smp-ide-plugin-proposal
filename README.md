# [WIP] Proposal: SMP IDE Plugin

Author: [@salsabiilashifa11](https://github.com/salsabiilashifa11)<br>
Upstream issue: [service-mesh-performance/service-mesh-performance#379](https://github.com/service-mesh-performance/service-mesh-performance/issues/379)

## Contents
[1. Background](!#1.-background)<br>
[2. Objectives](!#2.-objectives)<br>
[3. Proposal](!#3.-proposal)<br>
[4. Initial Experimentation](!#4.-initial-experimentation)<br>

## 1. Background
Currently, users can define SMP profile and models using YAML or JSON files on their own IDEs. However there isn't currently any IDE extensions that can improve developer experience when working with these components. 

## 2. Objectives
As discussed in [this issue](https://github.com/service-mesh-performance/service-mesh-performance/issues/379), we want to provide an IDE plugin in the form of a language extension can enhance the developer experience when working with SMP components, such as SMP profiles and model definitions. 

## 3. Proposal
To accomplish the goals mentioned above, it is proposed to create a VS Code extension with the following capabilities:
1. Custom Language Tooling <br>
a. Syntax Highlighting <br>
b. Autocompletion <br>
c. Tooltip Component Descripion <br>
2. Model Definition Preview
3. Extension Update Automation

### 3.1. Custom Language Tooling
The custom language tooling capability will enhance editing experience for SMP profile and model definitions in the IDE.  

#### 3.1.1. Alternative 1: Using VS Code API 
VS Code has a set of API endpoints that can be directly utilized for defining custom language tooling extensions. Syntax highlighting functionality in VS Code extensions can be achieved using the `Declarative language feature`. The language structure, in this case, SMP component definitions will be defined in static configuration files.

Autocompletion can be achieved in VS Code by using the `Programmatic language feature` in VS Code's language extension. Specifically, this can be impemented by calling the `languages.registerCompletionItemProvider` endpoint provided by the VS Code API. 

#### 3.1.2. Alternative 2: Using a Language Server
Language servers can implement everything that can be implemented using VS Code's API and more. It will be implemented following Microsoft's defined [Language Server Protocol (LSP)](https://microsoft.github.io/language-server-protocol/). The idea of using a language server is to decouple the implementation of the language tooling definitions and the code editor. This makes it possible to integrate the defined language tooling with multiple language editors more easily. 

The language server itself can be written in any language, and can be built on top of existing packages such as the [yaml-language-server](https://www.npmjs.com/package/yaml-language-server) to make SMP specific language server implementation a lot easier. By utilizing existing language servers, we can simply define our custom schemas.

![SMP drawio (1)](https://github.com/salsabiilashifa11/smp-ide-plugin-proposal/assets/68501885/1b070933-d1f7-449b-adeb-da153aafac5c)

### 3.2. [WIP] Model Definition Preview
Model and profile definition previews can be implemented using VS Code's Webview API. The idea is to generate an HTML representation of the model definitions as the user types it in to send to Webview. 
> Detailed implementation design will be added later.

### 3.3. [WIP] Extension Update Automation
Seeing that Meshery and SMP are both still very active projects and there will very likely be new model definitions or updates to existing models, we can include the extension update as a part of the CI/CD pipeline so that the extension will always stay up to date with the actual state of the projects. 

## 4. Initial Experimentation

The following profile definition was attempted to be written in VS Code using the defined schema that can later be embedded into the language server. 

```
smp_version: v0.0.1
name: Sample Test
labels:
  env: stg
clients:
  - internal: false
    load_generator: nighthawk
    protocol: 1
    connections: 2
    rps: 10
    headers: {}
    cookies: {}
    body: ''
    content_type: ''
    endpoint_urls:
      - https://localhost:3000
duration: 30s
```

https://github.com/salsabiilashifa11/smp-ide-plugin-proposal/assets/68501885/c81053c2-338b-4b45-9400-1f6cc0dabdaf
