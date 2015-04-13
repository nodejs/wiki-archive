# Introduction

Node.js depends on the OpenSSL library to implement its crypto and TLS/SSL features.

However, Node.js doesn't use OpenSSL's standard build process. Instead, a custom build process based on GYP that uses OpenSSL's source is maintained in parallel of OpenSSL's original build process. In addition to that, because the state of OpenSSL's support for Node.js supported platforms varies over time, it is sometimes necessary to maintain additional patches that fix some important issues.

For these two reasons, upgrading the OpenSSL version used by Node.js is not trivial. Some knowledge of how OpenSSL is built and embedded within the node binary and of the existing floating patches is necessary. This document describes both of these aspects so that upgrading the OpenSSL version used by a given version of Node.js does not have to be done by the few people with such knowledge.

## Floating patches

Following is the list of the current patches that are floated on top of OpenSSL's source:
* 2b21c45f75043f8e5728650e24a4e972ade18cf1
* 7817fbd692120887619d07228882dd19461109b6
* c4b9be7c5a97b9cac99cd599dbd995da556a5a17
* 6b97c2e98627b5189e01b871f9130b5efc41988d