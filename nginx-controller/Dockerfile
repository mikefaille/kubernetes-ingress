FROM nginx:1.11.1

# forward nginx access and error logs to stdout and stderr of the ingress
# controller process
RUN ln -sf /proc/1/fd/1 /var/log/nginx/access.log \
	&& ln -sf /proc/1/fd/2 /var/log/nginx/error.log

COPY main.go nginx/ingress.tmpl nginx/nginx.conf.tmpl /

RUN rm /etc/nginx/conf.d/*

RUN apt update && apt install curl && \
    cd /nginx-ingress && \
    curl -O https://storage.googleapis.com/golang/go1.6.2.linux-amd64.tar.gz && \
    tar xvfp go1.6.2.linux-amd64.tar.gz && \
    export GOROOT=$(pwd)/go && \
    go build . && \
    apt remove curl && \
    apt-get clean && \
    rm -R go/ go1.6.2.linux-amd64.tar.gz

ENTRYPOINT ["/nginx-ingress"]
