FROM openjdk:8-stretch

LABEL maintainer="dotiendat1751998 <dotiendat1751998@gmail.com>" version="9.7.0"

ARG JIRA_VERSION=9.7.0
# Production: jira-software jira-core
ARG JIRA_PRODUCT=jira-software
ARG AGENT_VERSION=1.3.3
ARG MYSQL_DRIVER_VERSION=8.0.22
#add new
ARG GLIBC_VERSION=2.29-r0
ARG POSTGRE_DRIVER_VERSION=42.6.0

ENV JIRA_USER=jira \
    JIRA_GROUP=jira \
    JIRA_HOME=/var/jira \
    JIRA_INSTALL=/opt/jira \
    JVM_MINIMUM_MEMORY=1g \
    JVM_MAXIMUM_MEMORY=4g \
    JVM_CODE_CACHE_ARGS='-XX:InitialCodeCacheSize=1g -XX:ReservedCodeCacheSize=2g' \
    AGENT_PATH=/var/agent \
    AGENT_FILENAME=atlassian-agent.jar

ENV JAVA_OPTS="-javaagent:${AGENT_PATH}/${AGENT_FILENAME} ${JAVA_OPTS}"

RUN mkdir -p ${JIRA_INSTALL} ${JIRA_HOME} ${AGENT_PATH} \
&& curl -o ${AGENT_PATH}/${AGENT_FILENAME}  https://github.com/tiendat1751998/jiradocker/raw/master/atlassian-agent.jar -L \
&& curl -o ${JIRA_INSTALL}/lib https://github.com/tiendat1751998/jiradocker/raw/master/postgresql-42.6.0.jar -L \
&& curl -o /tmp/atlassian.tar.gz https://product-downloads.atlassian.com/software/jira/downloads/atlassian-${JIRA_PRODUCT}-${JIRA_VERSION}.tar.gz -L \
&& tar xzf /tmp/atlassian.tar.gz -C ${JIRA_INSTALL}/ --strip-components 1 \
&& rm -f /tmp/atlassian.tar.gz \
&& curl -o ${JIRA_INSTALL}/lib/mysql-connector-java-${MYSQL_DRIVER_VERSION}.jar https://repo1.maven.org/maven2/mysql/mysql-connector-java/${MYSQL_DRIVER_VERSION}/mysql-connector-java-${MYSQL_DRIVER_VERSION}.jar -L \
&& echo "jira.home = ${JIRA_HOME}" > ${JIRA_INSTALL}/atlassian-jira/WEB-INF/classes/jira-application.properties

RUN export CONTAINER_USER=$JIRA_USER \
&& export CONTAINER_GROUP=$JIRA_GROUP \
&& groupadd -r $JIRA_GROUP && useradd -r -g $JIRA_GROUP $JIRA_USER \
&& chown -R $JIRA_USER:$JIRA_GROUP ${JIRA_INSTALL} ${JIRA_HOME}/ ${AGENT_PATH}

#thêm mới
  && export GLIBC_DOWNLOAD_URL=https://github.com/sgerrand/alpine-pkg-glibc/releases/download/$GLIBC_VERSION \
    && export GLIBC_BIN=glibc-bin-$GLIBC_VERSION.apk           \
    && export GLIBC_I18N=glibc-i18n-$GLIBC_VERSION.apk         \
    && apk add --update --no-cache --upgrade                   \
        ssl_client                                             \
        ca-certificates                                        \
    && wget -O $GLIBC_BIN $GLIBC_DOWNLOAD_URL/$GLIBC_BIN       \
    && wget -O $GLIBC_I18N $GLIBC_DOWNLOAD_URL/$GLIBC_I18N     \
    && apk add                                                 \
          --update                                             \
          --no-cache                                           \
           bash                                                \
           su-exec                                             \
           gzip                                                \
           nano                                                \
           tini                                                \
           wget                                                \
           xmlstarlet                                          \
           $GLIBC_BIN                                          \
           $GLIBC_I18N                                         \
           fontconfig                                          \
           msttcorefonts-installer                             \
           ttf-dejavu                                          \
           ghostscript                                         \
           graphviz                                            \
           motif                                                \
           vim


VOLUME $JIRA_HOME
USER $JIRA_USER
WORKDIR $JIRA_INSTALL
EXPOSE 8080

ENTRYPOINT ["/opt/jira/bin/start-jira.sh", "-fg"]
