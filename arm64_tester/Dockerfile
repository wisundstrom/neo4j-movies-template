FROM arm64v8/ubuntu

MAINTANER Your Name "youremail@domain.tld"

RUN apt-get update -y && \
    apt-get install -y python-pip python-dev

RUN apt-get install dnsutils && \
    apt-get install iputils-ping
    
RUN pip install --no-cache-dir neo4j py2neo requests
 
CMD [ "bash" ]

