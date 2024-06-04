---
title: "TypeSpec Native Extension"
toc_min_heading_level: 2
toc_max_heading_level: 3
---

# Motivation

### Current API Modeling Approach and Challenges

The ACS native SDK team currently utilizes ApiGen and its associated tooling to model the public interfaces of ACS SDK APIs. This approach defines a set of C# types, collectively called API Models, that represent the functionalities available across supported platforms (Android, iOS, WinRT). While ApiGen ensures consistency in APIs between these platforms, it heavily relies on C# primitives and language-specific extensions to bridge the gap with native platforms. This often necessitates exceptions and compromises Azure API guideline compliance. Consequently, ACS native SDKs have diverged from the rest of the Azure SDK family, leading to a steeper learning curve for developers and hindering code reuse. 
Additionally, this modeling method operates independently of the ACS Web SDK.

### TypeSpec: A Path Toward Consistency

TypeSpec presents a promising solution for ACS SDKs. It offers a consistent and expressive way to define code interfaces and behavior, facilitating smoother onboarding to the Azure SDK family. This would enable ACS SDKs to benefit from the same review, implementation, and quality control processes as other Azure SDKs.

### Addressing AutoRest Limitations

Developing a completely new emitter and code generator for our native extension would be a significant undertaking. While AutoRest, the popular TypeSpec code generator for Azure SDKs, initially seemed like a good fit for customization, its limitations prevent its direct use. AutoRest primarily targets RESTful APIs with HTTP communication, making it unsuitable for native SDK development. Furthermore, AutoRest lacks support for essential features in native SDKs, such as C# property accessors (getters and setters) and templated events.

### Proposed Solution: A Native TypeSpec Extension

Our proposed solution is a native extension for TypeSpec. This extension, akin to AutoRest's support for RESTful APIs and HTTP, would focus on generating Azure-compliant native SDKs. It would achieve this by introducing a set of decorators and compiler/emitter helpers specifically designed for native development. This extension has the potential to be expanded in the future to support additional languages like TypeScript, Swift, React Native, and C/C++.


# Scope

The scope of this proposal is to design and implement a native extension of TypeSpec that can generate code for native SDKs. The extension will have the following features:

- Support common languages by allowing developers to specify the protocol and the serialization format in the specification file. Tentative plan is to prioritize shipping C# on Windows.
- Support properties, events, callbacks, generics, and async/await in the generated code.
- Support inheritance, polymorphism, and interfaces in the generated code.
- Support validation, error handling, and logging in the generated code.
- Support customization and extensibility in the generated code.
- Support code generation for other languages, such as TypeScript, Swift and C/C++, by allowing developers to specify the target language and the code template in the specification file.
- Platform interop is still TBD.

# Design
The design of the native extension of TypeSpec will follow the same principles and architecture of TypeSpec and AutoRest, but with some modifications and enhancements specifically in AutoRest. The design will consist of the following components (using dotnet SDK for example):

- @typespec/native

&nbsp;&nbsp;&nbsp;&nbsp;This library is similar to @typespec/http, @typespec/rest, and @typespec/openapi3 libraries to help define a non-RESTful API, this extension is meant to be plugged into TypeSpec next to @typespec/http etc.  
&nbsp;&nbsp;&nbsp;&nbsp;This is where native decorators are defined, such as @property, @getter, @setters and @event.

- @azure-tools/typespec-native-csharp

This is the emitter for generating the tspCodeModel.json, which is in turn used by C# code generator to produce the final C# SDK project.

- @AutoNative/csharp
This is the C# code generator


# TypeSpec Native ACS Gist:
https://gist.github.com/jowang-msft/652c650fae2caf347108f54de352f92a