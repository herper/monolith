ARG REDIS_VERSION
FROM redis:${REDIS_VERSION}

# Fix PID file not having permissions
RUN mkdir /var/run/redis && chown redis:redis /var/run/redis

CMD ["redis-server", "/usr/local/etc/redis/redis.conf"]
