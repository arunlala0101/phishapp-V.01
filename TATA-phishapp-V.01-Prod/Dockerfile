
# Build Golang binary
FROM golang:1.15.2 AS build-golang

WORKDIR /SQ-APP/phishapp-V.01/TATA-phishapp-V.01-Prod
COPY . .RUN go get -v && go build -v


# Runtime container
FROM debian:stable-slim

RUN useradd -m -d /opt/gophish -s /bin/bash app

RUN apt-get update && \
	apt-get install --no-install-recommends -y jq libcap2-bin && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


WORKDIR /opt/gophish
COPY --from=build-golang /SQ-APP/phishapp-V.01/TATA-phishapp-V.01-Prod ./
COPY --from=build-js /build/static/js/dist/ ./static/js/dist/
COPY --from=build-js /build/static/css/dist/ ./static/css/dist/
COPY --from=build-golang /SQ-APP/phishapp-V.01/TATA-phishapp-V.01-Prod/gophish/gophish/config.json ./
RUN chown app. config.json

RUN setcap 'cap_net_bind_service=+ep' /opt/gophish/gophish

USER app
RUN sed -i 's/127.0.0.1/0.0.0.0/g' config.json
RUN touch config.json.tmp

EXPOSE 3333 8080 8443 80

CMD ["./docker/run.sh"]