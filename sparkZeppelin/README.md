# Setup:

Assumes that docker is installed.  The container downloads Spark, so it builds a little slowly.  

## Build container:

docker build -t sparkzeppelin . 

docker run --name sparky -p 127.0.0.1:8889:8889 -td sparkzepp

## Run 

http://localhost:8889/#/notebook/2D8WZWAFZ
