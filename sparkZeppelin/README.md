# Setup:

Assumes that docker is installed.  The container downloads Spark, so it builds a little slowly.  
Has a few placeholders for adding in aws config for localstack

## Build container:

cd sparkZeppelin && docker build -t sparkzepp . 

docker run --name sparky -p 127.0.0.1:8889:8889 -td sparkzepp

## Run 

http://localhost:8889/#/notebook/2A94M5J1Z
