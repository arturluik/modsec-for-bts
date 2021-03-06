# Sample Modsecurity proxy config for Blue team excercises


How to use the image?

```
version: "2.4"

services:
  web:
    # Add your custom image here!!!
    image: php:7-apache
    depends_on:
      - modsec
      - db
    environment:
      - DB_HOST=db
    networks:
      proxy: {}

  db:
    image: postgres:12
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=<pass>
  #  volumes:
  #    - ./db:/var/lib/postgresql/data
  #    - ./dump.sql:/docker-entrypoint-initdb.d/dump.sql
    networks:
      proxy: {}

  ipv6:
    image: robbertkl/ipv6nat
    restart: unless-stopped
    network_mode: "host"
    privileged: true
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /lib/modules:/lib/modules:ro

  modsec:
    image: waf:latest
    ports:
      - "80:80"
      - "443:443"
    networks:
      - internal
      - proxy
    environment:
      - BACKEND_PORT80=http://web:3000
      - BACKEND_PORT443=http://web:3000
    #  - PROXY_SSL_CERT=/keys/cert.pem
    #  - PROXY_SSL_CERT_KEY=/keys/privkey.pem
      - CSP="default-src 'self' custom.domain.ee"

    # volumes:
    #  - ./REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf:/etc/modsecurity.d/owasp-crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
    #  - ./RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf:/etc/modsecurity.d/owasp-crs/rules/RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf
    #  - ./x.org/cert.pem:/keys/cert.pem
    #  - ./x.org/privkey.pem:/keys/privkey.pem
    #
    #

networks:
  internal:
    enable_ipv6: true
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: fd00:dead:beef::/48
  proxy:
    external: true
```