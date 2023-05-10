# Docker-compose Keycloak

Examples of docker-compose using keycloak(jboss and quay.io version) with postgres

## Quay.io (Keycloak version 21.0.1
```yaml
version: '3'

volumes:
  postgres_data:
    driver: local

services:
  postgres:
    image: postgres:latest
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: 123456
    healthcheck:
      test: "exit 0"
    ports:
      - 5432:5432
    networks:
      - keycloak_demo

  keycloak:
    image: quay.io/keycloak/keycloak:21.0.1
    command: start-dev
    environment:
      KC_DB: postgres
      KC_DB_URL_HOST: postgres
      KC_DB_URL_DATABASE: keycloak
      KC_DB_PASSWORD: 123456
      KC_DB_USERNAME: keycloak
      KC_DB_SCHEMA: public
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: password
    ports:
      - 8081:8080
    depends_on:
      postgres:
        condition: service_healthy
    networks:
      - keycloak_demo
networks:
  keycloak_demo:
    driver: bridge
```

## Jboss (Keycloak version 16.1.1)
```yaml
version: '3'

volumes:
  postgres_data:
    driver: local

services:
  postgres:
    image: postgres:latest
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: 123456
    ports:
      - 5432:5432
  
  keycloak:
    image: jboss/keycloak:latest
    environment:
      DB_VENDOR: POSTGRES
      DB_ADDR: postgres
      DB_DATABASE: keycloak
      DB_USER: keycloak
      DB_SCHEMA: public
      DB_PASSWORD: 123456
      KEYCLOAK_USER: admin
      KEYCLOAK_PASSWORD: password
    ports:
      - 8080:8080
    depends_on:
      - postgres
```
