FROM redis:3.0

COPY sentinel.conf /usr/local/etc/redis/sentinel.conf
CMD [ "redis-server", "/usr/local/etc/redis/sentinel.conf", "--sentinel" ]

EXPOSE 9002
