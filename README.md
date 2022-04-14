# Sample Modsecurity proxy config for Blue team excercises


How to use the image?

```
version: "3"

services:
  web:
    image: www
    depends_on:
      - modsec
      - db
    environment:
      - DB_HOST=db
      - DB_USER=postgres
      - DB_PASS=<pass>

  db:
    image: postgres:12
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=<pass>
    ports:
      - '5432:5432'
    volumes:
      - ./db:/var/lib/postgresql/data
      - ./dump.sql:/docker-entrypoint-initdb.d/dump.sql
  modsec:
    image: waf:latest
    ports:
      - "80:80"
      - "443:443"

    environment:
      - BACKEND_PORT80=http://web:3000
      - BACKEND_PORT443=http://web:3000
      - PROXY_SSL_CERT=/keys/cert.pem
      - PROXY_SSL_CERT_KEY=/keys/privkey.pem

    volumes:
    #  - ./REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf:/etc/modsecurity.d/owasp-crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
    #  - ./RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf:/etc/modsecurity.d/owasp-crs/rules/RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf
      - ./bob.19.berylia.org/cert.pem:/keys/cert.pem
      - ./bob.19.berylia.org/privkey.pem:/keys/privkey.pem
    #
```