Utilizando Jgroups
http://www.jgroups.org/

JGroups é um kit de ferramentas para mensagens confiáveis. Ele pode ser usado para criar clusters cujos nós podem enviar mensagens uns aos outros. As principais características incluem

    Criação e exclusão de cluster. Os nós do cluster podem ser espalhados por LANs ou WANs
    Entrada e saída de clusters
    Detecção de associação e notificação sobre nós de cluster ingressados/esquerdos/com falha
    Detecção e remoção de nós com falha
    Envio e recebimento de mensagens nó-a-cluster (ponto-a-multiponto)
    Envio e recebimento de mensagens nó a nó (ponto a ponto)

===================================================

Rodar em docker
    docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:21.0.2 start-dev

===================================================

Reclamando do arquivo:
    /opt/infinispan/server/conf/infinispan-keycloak.xml

Corrigido com copia na imagem:
    COPY --chown=ispn:ispn ./infinispan-keycloak.xml /opt/infinispan/server/conf/infinispan-keycloak.xml

===================================================

Arquivo faltando:
    ERROR: /opt/keycloak/conf/jgroups.p12 (No such file or directory)

Esse arquivo precisa ser enviado para que possamos validar o ambiente de testes.

===================================================

Invalid credentials

    keycloak             | 2023-04-19 13:21:23,565 WARN  [org.infinispan.HOTROD] (Thread-0) ISPN004005: Error received from the server: java.lang.SecurityException: ISPN028027: Invalid credentials
    keycloak             | javax.security.sasl.SaslException: ELY05051: Callback handler does not support credential acquisition [Caused by org.wildfly.security.auth.callback.FastUnsupportedCallbackException: javax.security.auth.callback.PasswordCallback@7b092f9d]

Problema ocorre ao trocar senhas no docker-compose, entra em conflito com o arquivo cache-ispn-remote.xml que possui senhas no corpo.

                    <authentication>
                        <digest username="${env.KEYCLOAK_REMOTE_ISPN_USERNAME:keycloak}"
                                password="${env.KEYCLOAK_REMOTE_ISPN_PASSWORD:password}"
                                realm="default"/>
                    </authentication>

===================================================

Ao habilitar o command no docker-compose a entrada no painel de adm fica em loop - provavelmente pela falta do arquivo jgroups

    "--spi-sticky-session-encoder-infinispan-should-attach-route=false"

    keycloak             | 2023-04-19 14:36:13,248 WARN  [org.jgroups.protocols.UDP] (jgroups-8,5c1b3ae772b9-30325) JGRP000012: discarded message from different cluster REMOTE (our cluster is ISPN). Sender was 5da6480d-ef29-4796-af07-daf3b4f8f706 (received 16 identical messages from 5da6480d-ef29-4796-af07-daf3b4f8f706 in the last 62042 ms)

===================================================

Subindo ultima versao funcional...








