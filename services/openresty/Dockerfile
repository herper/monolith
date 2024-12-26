ARG OPENRESTY_VERSION
FROM openresty/openresty:${OPENRESTY_VERSION:-1.27.1.1-0-alpine}

ARG ALPINE_MIRROR
RUN if [ ${ALPINE_MIRROR} != "" ]; then \
        alpine_mirror_url="${ALPINE_MIRROR}"; \
    else \
        alpine_mirror_url="dl-cdn.alpinelinux.org"; \
    fi; \
    sed -i "s/dl-cdn.alpinelinux.org/${alpine_mirror_url}/g" /etc/apk/repositories

RUN apk update && apk --no-cache add tzdata logrotate

COPY logrotate/openresty /etc/logrotate.d/openresty

RUN echo "0 0 * * * logrotate -f /etc/logrotate.d/openresty" >> /etc/crontabs/root

CMD ["sh", "-c", "crond -b && /usr/local/openresty/bin/openresty -g 'daemon off;'"]
