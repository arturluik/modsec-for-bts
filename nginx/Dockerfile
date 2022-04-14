FROM owasp/modsecurity-crs:3.3.2-nginx-alpine

COPY conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/nginx.conf
COPY modsecurity-override.conf /etc/modsecurity.d/modsecurity-override.conf

RUN cp /etc/nginx/templates/conf.d/modsecurity.conf.template /etc/nginx/conf.d/modsecurity.conf