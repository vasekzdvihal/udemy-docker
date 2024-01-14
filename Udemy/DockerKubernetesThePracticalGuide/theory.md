### Docker & Kubernetes: The Practical Guide

#### 1: Getting Started

Build image based on Dockerfile (folder you are in have to have Dockerfile)
``` terminal
   docker build .
```

You get image ID. On windows (all after sha256:)

` => => writing image sha256:e0a88a95756d4698b9a185dc5aa419b72c97f0bfbdfd350e06b979cfa79e3b1f    `

on mac (last string)

` Succesfully built d2cc7b04fb0a  `

Then you can run it

``` terminal
 docker run e0a88a95756d4698b9a185dc5aa419b72c97f0bfbdfd350e06b979cfa79e3b1f
```
-p publishes port (we have webapp containezed in example)
```
 -p 3000:3000
```