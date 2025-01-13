FROM quay.io/keycloak/keycloak:22.0.1 as builder

#setting timezone Asia/Jakarta
ENV TZ=Asia/Jakarta
USER root
RUN ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime

WORKDIR /opt/keycloak
# for demonstration purposes only, please make sure to use proper certificates in production instead
RUN keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:22.0.1
COPY --from=builder /opt/keycloak/ /opt/keycloak/

# change these values to point to a running postgres instance
ENV KC_DB=postgres
ENV KC_DB_URL=jdbc:postgresql://192.168.1.200:5432/keycloak
ENV KC_DB_USERNAME=postgres
ENV KC_DB_PASSWORD=s@mAZsWuP@6RGi

# Enable health and metrics support
ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true
ENV KC_TRANSACTION_XA_ENABLED=false

ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start-dev"]
