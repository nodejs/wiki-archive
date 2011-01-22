Many people are running code on the stable branch v0.2. Soon the unstable branch, v0.3, will become stable. You can help the community by documenting what API changes were made between the two.

## C++ API

* The internal node::Buffer class had dramatic changes early on in the v0.3. Any addons which used node:Buffer will require heavy rewrite. You might find this helpful https://github.com/pkrumins/node-png/blob/791d4c6df1402daa15dc7930f084d95c48e63c98/src/buffer_compat.h

## JavaScript API

## Internal C++ API (inside of NODE_ROOT/src/*.cc)

## Internal Javascript API (inside of NODE_ROOT/lib/*.js

 
