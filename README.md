## Introduction

Protobuf-cpp11 adds C++11/C++14 features to [Google's Protocol Buffers](https://github.com/google/protobuf).

Protocol Buffers is widely used in Google internally and many projects are still using C++98 compiler, so at current state it can be a slow process for Google to switch to C++11 for Protocol Buffers.

This repository is to enable C++11 features for people who already using C++11 compiler in their projects.

## Installation and Usage
All new features are added by introducing new options without breaking backward compatibility, so [installation](https://github.com/google/protobuf/blob/master/src/README.md) and [usage](https://developers.google.com/protocol-buffers/) are completely the same as Google's Protocol Buffers.

_Note that C++11 compiler is not mandatory. These new features will only be activated by options explicitly, so you will only need C++11 compiler when enabling C++11 features._

## Features

* [scoped enum](#scoped-enum)

### scoped enum
(like enum class in C++11)

_**option name**_: scoped

By default enum is not scoped, for example compiling following message will generate an error:

    message Counter {
      enum Status {
        start = 0;
        stop = 1;
      }
      required int32 stop = 1;  // error: duplicated name defined in enum Status
    }

Thanks to [Warren Falk](https://github.com/warrenfalk), now it is supported by a new option "scoped" in enum:

    message Counter {
      enum Status {
        option scoped = true;
        start = 0;
        stop = 1;
      }
      required int32 stop = 1;
    }

And you can define same name in different enums with different values, like enum class in C++11:

    message Counter {
      enum Status {
        option scoped = true;
        start = 0;
        stop = 1;
      }
      enum Action {
        option scoped = true;
        stop = 42;
      }
      required int32 stop = 1;
      optional Status status = 2 [default = stop];  // by default status will set to 1
      optional Action action = 3 [default = stop];  // by default action will set to 42
    }

Note: This option doesn't require C++11 support, it just scopes the enum value names to the enum instead of the parent of the enum, here is [detailed discussion](https://github.com/google/protobuf/issues/1079).

## Todo List

* move contructor - [discussion link](https://groups.google.com/forum/#!topic/protobuf/QdHPjwmUgXw)
* final class
