FROM shopify/toxiproxy

RUN apk update && apk add curl

#
# install jq to parse json within bash scripts
RUN curl -o /usr/local/bin/jq http://stedolan.github.io/jq/download/linux64/jq && \
  chmod +x /usr/local/bin/jq

RUN mkdir /config

CMD ["-host=0.0.0.0", "-config=/config/toxiproxy.json", "-port=6666", "-runtime-metrics"]
