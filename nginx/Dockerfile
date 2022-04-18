FROM owasp/modsecurity-crs:3.3.2-nginx-alpine

ENV PARANOIA=1 \
    BLOCKING_PARANOIA=1 \
    ANOMALY_INBOUND=5 \
    ANOMALY_OUTBOUND=4 \
    REPORTING_LEVEL=2 \
    NGINX_KEEPALIVE_TIMEOUT=60s \
    ERRORLOG=/var/log/nginx/error.log \
    LOGLEVEL=warn \
    USER=nginx \
    PORT=80 \
    SERVERNAME=locahost \
    WORKER_CONNECTIONS=1024 \
    MODSEC_RULE_ENGINE=on \
    MODSEC_REQ_BODY_ACCESS=on \
    MODSEC_REQ_BODY_LIMIT=200000000 \
    MODSEC_REQ_BODY_NOFILES_LIMIT=131072 \
    MODSEC_RESP_BODY_ACCESS=on \
    MODSEC_RESP_BODY_LIMIT=1048576 \
    MODSEC_PCRE_MATCH_LIMIT=100000 \
    MODSEC_PCRE_MATCH_LIMIT_RECURSION=100000 \
    CSP="default-src 'self'"

RUN apk add php7-fpm

COPY conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY conf.d/unexpected_403.php /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
COPY modsecurity-override.conf /etc/modsecurity.d/modsecurity-override.conf
COPY start.sh /start.sh

RUN chmod +x /start.sh

RUN cp /etc/nginx/templates/conf.d/modsecurity.conf.template /etc/nginx/conf.d/modsecurity.conf

CMD ["/start.sh"]