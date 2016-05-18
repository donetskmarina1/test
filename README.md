[![Build Status](https://travis-ci.org/VirgilSecurity/virgil-crypto.svg?branch=master)](https://travis-ci.org/VirgilSecurity/virgil-crypto)
[![GitHub license](https://img.shields.io/badge/license-BSD%203--Clause-blue.svg)](https://raw.githubusercontent.com/VirgilSecurity/virgil-crypto/master/LICENSE)
[![Documentation Developers](https://img.shields.io/badge/docs-developers-green.svg)](https://virgilsecurity.com/api-docs)
[![Documentation Doxygen](https://img.shields.io/badge/docs-doxygen-blue.svg)](http://VirgilSecurity.github.io/virgil-crypto)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/4943/badge.svg)](https://scan.coverity.com/projects/virgilsecurity-virgil-crypto)

# Library: Virgil Crypto

- [Introduction](#introduction)
- [Supported languages and platforms](#supported-languages-and-platforms)
- [Library purposes](#library-purposes)
- [Where library can be used](#where-library-can-be-used)
- [Build prerequisites](#build-prerequisites)
- [Simple build](#simple-build)
- [Multiarch build](#multiarch-build)
- [Support](#support)

## Introduction
Welcome to Virgil!

Virgil is a stack of security libraries (ECIES with Crypto Agility wrapped in Virgil Cryptogram) and all the necessary
infrastructure to enable seamless, end-to-end encryption for any application, platform or device.
See below for currently available languages and platforms.
Get in touch with us to get preview access to our key infrastructure.

Virgil Security, Inc., guides software developers into the forthcoming security world in which everything will be encrypted (and passwords will be eliminated).  In this world, the days of developers having to raise millions of dollars to build a secure chat, secure email, secure file-sharing, or a secure anything have come to an end.  Now developers can instead focus on building features that give them a competitive market advantage while end-users can enjoy the privacy and security they increasingly demand.

## Supported languages and platforms

| Language | Supported OS               | Usage                                                                                                                                                                                                                        |
|----------|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| C++      | ANY                        | [Tutorial](https://github.com/VirgilSecurity/virgil-sdk-cpp/blob/release/tutorial/virgil_crypto.md),  [Reference API](http://virgilsecurity.github.io/virgil-crypto/)                                                        |
| PHP      | Unix, Linux, OS X          |                                                                                                                                                                                                                              |
| Python   | Unix, Linux, OS X          | [Tutorial](https://github.com/VirgilSecurity/virgil-sdk-python/blob/master/Docs/crypto.md)                                                                                                                                   |
| Ruby     | Unix, Linux, OS X          |                                                                                                                                                                                                                              |
| Java     | Unix, Linux, OS X, Windows | [Tutorial](https://github.com/VirgilSecurity/virgil-sdk-java-android/blob/master/docs/crypto.md)                                                                                                                             |
| .NET     | Unix, Linux, OS X, Windows | [Tutorial](https://github.com/VirgilSecurity/virgil-sdk-net/blob/master/Docs/crypto.md), [Reference API](https://github.com/VirgilSecurity/virgil-sdk-net/blob/v3-docs/Crypto/Source/Virgil.Crypto.Wrapper/reference-api.md) |
| AsmJS    | Unix, Linux, OS X, Windows | [Tutorial](https://github.com/VirgilSecurity/virgil-crypto-javascript)                                                                                                                                                       |
| NodeJS   | Unix, Linux, OS X, Windows | [Tutorial](https://github.com/VirgilSecurity/virgil-crypto-javascript)                                                                                                                                                       |                      

## Library purposes
* Encrypt data;
* Decrypt data;
* Sign data;
* Verify data.

## Where library can be used
* on the client-side application;
* on the server-side application.

## Build prerequisites

The page lists the prerequisite packages which need to be installed on the different platforms to be able to configure and to build Virgil Crypto Library.

* Compiler:
  - ```g++```, or
  - ```clang++```, or
  - ```msvc++``` (version >= 12.0)
* Build tools:
  - ```cmake``` (version >= 3.2)
  - ```make```
* Other tools:
  - ```git```
  - ```swig``` (version >= 3.0.7), optional for C++ build
  - ```doxygen``` (optional)


## Simple build

This section describes how to build Virgil Crypto Library for а particular language.

### Step 1 - Choose target language

<a name="table1"></a>
Table 1 - Supported languages

|Language| Supported OS               | Dependencies          | Build parameters | Environment | Binary |
|--------|----------------------------|-----------------------|------------------|-------------|--------|
| C++      | ANY                        |                       | LANG=cpp         |             |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/cpp/)|
| PHP      | Unix, Linux, OS X          | php, php5-dev         | LANG=php         |             |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/php/)|
| Python   | Unix, Linux, OS X          | python                | LANG=python      |             |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/python/)
| Ruby     | Unix, Linux, OS X          | ruby, ruby-dev        | LANG=ruby        |             |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/ruby/)|
| Java     | Unix, Linux, OS X, Windows\* | Java JDK 1.6          | LANG=java        | JAVA_HOME   |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/java/)|
| .NET     | Unix, Linux, OS X, Windows\* | .NET 2.0, or mono 2.0 | LANG=net         |             |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/net/)|
| AsmJS    | Unix, Linux, OS X, Windows\* | Emscripten 1.35       | LANG=asmjs       | EMSDK_HOME  |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/asmjs/)|
| NodeJS   | Unix, Linux, OS X, Windows\* |                       | LANG=nodejs      |             |[CDN](https://cdn.virgilsecurity.com/virgil-crypto/nodejs/)|

>\* External dependencies for Windows binaries:
>	- msvcp140.dll
>	- vcruntime140.dll

>These dependencies can be installed as a part of [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)

### Step 2 - Configure environment

1. Open Terminal.
1. Check that all the tools which are listed in the [build prerequisite](#build-prerequisite) are available there.
1. Set environment variables according to the [table above](#table1).

### Step 3 - Get source code

```shell
> git clone https://github.com/VirgilSecurity/virgil-crypto.git
```

### Step 4 - Build

Replace ```{{LANG}}``` placeholder to the corresponding value from the [table above](#table1).

```shell
> cd virgil-crypto
> cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DLANG={{LANG}}
> cmake --build _build --target install
```

> Note, if you are using ```-DLANG=nodejs```, one of the next parameters can be appended:
>
> * ```-DLANG_VERSION=0.12.7```
> * ```-DLANG_VERSION=4.1.0```

## Multiarch build

This section describes how to build Virgil Crypto Library for multi architecture targets, which are packed inside the specific package:

* Apple OS X Framework
* Apple iOS Framework
* Apple WatchOS Framework
* Apple TVOS Framework
* Android Bundle as Jar archive
* Windows Bundle, as structured

### Step 1 - Choose target language and platform

<a name="table2"></a>
Table 2 - Supported languages and platforms

| Language| Platform |Host OS| Dependencies          | Build parameters        |Environment|Binary|
|---------|----------|-------|-----------------------|-------------------------|-----------|------|
| C++         | OS X     | OS X |                       | TARGET=osx              |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/cpp/virgil-crypto-1.6.0-cpp-osx-14.5-universal.tgz)|
| C++         | iOS      | OS X |                       | TARGET=ios              |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/cpp/virgil-crypto-1.6.0-cpp-ios-7.0.tgz)|
| C++         | WatchOS  | OS X |                       | TARGET=applewatchos     |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/cpp/virgil-crypto-1.6.0-cpp-applewatchos-2.0.tgz)|
| C++         | TVOS     | OS X |                       | TARGET=appletvos        |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/cpp/virgil-crypto-1.6.0-cpp-appletvos-9.0.tgz)|
| C++         | Windows  | Windows| msvcp140.dll\*, vcruntime140.dll\*                     | TARGET=cpp              |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/cpp/virgil-crypto-1.6.0-cpp-windows-6.3-x64.zip)|
| .NET        | iOS      | OS X | mono 2.0              | TARGET=net_ios          |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/net/virgil-crypto-1.6.0-mono-ios-7.0.tgz)|
| .NET        | WatchOS  | OS X | mono 2.0              | TARGET= net_applewatchos |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/net/virgil-crypto-1.6.0-mono-applewatchos-2.0.tgz)|
| .NET        | TVOS     | OS X | mono 2.0              | TARGET=net_appletvos    |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/net/virgil-crypto-1.6.0-mono-appletvos-9.0.tgz)|
| .NET        | Android  | *nix | Android NDK, mono 2.0 | TARGET=net_android      | ANDROID_NDK |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/net/virgil-crypto-1.6.0-mono-android-21.tgz)|
| .NET        | Windows  | Windows| .NET 2.0, msvcp140.dll\*, vcruntime140.dll\*              | TARGET=net              |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/net/virgil-crypto-1.6.0-net-windows-6.3.zip)|
| Java        | Android  | *nix | Android NDK           | TARGET=java_android     | ANDROID_NDK |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/java/virgil-crypto-1.6.0-java-android-21.tgz)|
| Java        | Windows  | Windows| Java JDK, msvcp140.dll\*, vcruntime140.dll\*              | TARGET=java             | JAVA_HOME   |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/java/virgil-crypto-1.6.0-java-windows-6.3-x64.zip)|
| NodeJS 0.12 | Windows  | Windows| msvcp140.dll\*, vcruntime140.dll\*                       | TARGET=nodejs-0.12.7    |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/nodejs/virgil-crypto-1.6.0-nodejs-0.12.7-windows-6.3-x64.zip)|
| NodeJS 4.1  | Windows  | Windows| msvcp140.dll\*, vcruntime140.dll\*                       | TARGET=nodejs-4.1.0     |             |[:arrow_down:](https://cdn.virgilsecurity.com/virgil-crypto/nodejs/virgil-crypto-1.6.0-nodejs-4.1.0-windows-6.3-x64.zip)|

>These dependencies can be installed as a part of [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)

### Step 2 - Configure environment

1. Open Terminal.
1. Check that all tools which are listed in the [build prerequisites](#build-prerequisites) are available there.
  * for Windows compiler should be MSVC;
  * for OS X build toolchain should be Xcode Toolchain.
1. Check that all dependencies from the [table above](#table2) are accessible.
1. Set environment variables according to the [table above](#table2).

### Step 3 - Get source code

```shell
> git clone https://github.com/VirgilSecurity/virgil-crypto.git
```

### Step 4 - Build

Replace ```{{TARGET}}``` placeholder to the corresponding value from the [table above](#table2).

Unix-like OS:

```shell
> cd virgil-crypto
> ./utils/build.sh {{TARGET}}
> ls ./install/{{TARGET}}
```

Windows OS:

```shell
> set MSVC_ROOT=c:\path\to\msvc\root
> set JAVA_HOME=c:\path\to\jdk
> cd virgil-crypto
> .\utils\build.bat {{TARGET}}
> dir .\install\{{TARGET}}
```


## Support
Email to: <support@VirgilSecurity.com>
