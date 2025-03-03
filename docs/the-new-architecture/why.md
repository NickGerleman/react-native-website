---
id: why
title: Why A New Architecture
---

import NewArchitectureWarning from '../\_markdown-new-architecture-warning.mdx';

<NewArchitectureWarning/>

The goal of the New Architecture is to solve some of the issues that afflicted the Old Architecture in terms of performance and flexibility. This section provides the basic context to understand the Old Architecture limitations and how it has been possible to overcome them with the New Architecture.

This is not a technical deep dive: for further technical information, refer to the [Architecture](/architecture/overview) tab of the website.

## Old Architecture's Issues

The Old Architecture used to work by serializing all the data that has to be passed from the JS layer to the native layer using a component called _The Bridge_. _The Bridge_ can be imagined as a bus where the producer layer sent some data for the consumer layer. The consumer could read the data, deserialize it and execute the required operations.

_The Bridge_ had some intrinsic limitations:

- **It was asynchronous:** one layer submitted the data to the bridge and asynchronously "waited" for the other layer to process them, even when this was not really necessary.
- **It was single threaded:** JS used to work on a single thread, therefore the computation that happened in that world had to be performed on that single thread.
- **It imposed extra overheads:** everytime one layer had to use the other one, it had to serialize some data. The other layer had to deserialize them. The chosen format was JSON, for its simplicity and human-readability, but despite being lightweight, it was a cost to pay.

## New Architecture's Improvements

The New Architecture dropped the concept of _The Bridge_ in favor of another communication mechanism: the _JavaScript Interface (JSI)_. The _JSI_ is an interface that allows a JavaScript object to hold a reference to a C++ and viceversa.

Once an object has a reference to the other one, it can directly invoke methods on it. So, for example, a C++ object can now asks a JavaScript object to execute a method in the JavaScript world and viceversa.

This idea allowed to unlock several benefits:

- **Synchronous execution:** it is now possibile to execute synchronously those functions that should not have been asynchronous in the first place.
- **Concurrency:** it is possible from JavaScript to invoke functions that are executed on different thread.
- **Lower overhead:** the New Architecture don't have to serialize/deserialize the data anymore, therefore there are no serialization taxes to pay.
- **Code sharing:** by introducing C++, it is now possible to abstract all the platform agnostic code and to share it with ease between the plaforms.
- **Type safety:** to make sure that JS can properly invoke methods on C++ objects and viceversa, a layer of code automatically generated has been added. The code is generated starting from some JS specification that must be typed through Flow or TypeScript.

These advantages are the foundations of the [TurboModule](pillars-turbomodules) system and a jumping stone to further enhancements. For example, it has been possible to develop a new renderer which is faster and more performant: [Fabric](/architecture/fabric-renderer) and its [Fabric Components](pillars-fabric-components).

## Further Reading

For a technical overview of the New Architecture, have a look at the [Architecture tab](/architecture/overview).

For more information on the Fabric Renderer, have a look at the [Fabric section](/architecture/fabric-renderer).
