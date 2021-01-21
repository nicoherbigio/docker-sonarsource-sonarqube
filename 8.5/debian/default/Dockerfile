# Assign aliases to the common images to improve readability and
# making sure that when a shared image needs to be updated, only a single line needs to be changed.

FROM sonarqube:8.5.1-community AS origin
FROM nicoherbigde/adoptopenjdk-openjdk:11.0.8 AS base

LABEL maintainer="Nico Herbig <nico@nicoherbig.io>"


# Add SonarSource SonarQube user and group to ensure that the IDs (UID and GID)
# are consistently assigned regardless of which additional dependencies are added.

ENV RUN_USER_ID=901 RUN_USER=sonarqube
ENV RUN_GROUP_ID=901 RUN_GROUP=sonarqube

RUN groupadd -r -g ${RUN_GROUP_ID} ${RUN_GROUP} && useradd -r -g ${RUN_GROUP} -u ${RUN_USER_ID} ${RUN_USER}


# Install additional required software packages

RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends bash ca-certificates fontconfig gosu libfreetype6 procps ttf-dejavu tzdata \
    && rm -rf /var/lib/apt/lists/*


# Install SonarSource SonarQube

ENV SONARQUBE_HOME=/opt/sonarqube
ENV SONAR_VERSION=8.5.1.38104

ENV SQ_DATA_DIR=/opt/sonarqube/data
ENV SQ_EXTENSIONS_DIR=/opt/sonarqube/extensions
ENV SQ_LOGS_DIR=/opt/sonarqube/logs
ENV SQ_TEMP_DIR=/opt/sonarqube/temp

COPY --from=origin --chown=${RUN_USER}:${RUN_GROUP} ${SONARQUBE_HOME} ${SONARQUBE_HOME}


# Configure SonarSource SonarQube

COPY --chown=${RUN_USER}:${RUN_GROUP} assets/*.sh ${SONARQUBE_HOME}/bin/

RUN set -ex \
    && chmod +x ${SONARQUBE_HOME}/bin/*.sh

EXPOSE 9000

WORKDIR ${SONARQUBE_HOME}

CMD ["bin/sonar.sh"]
ENTRYPOINT ["bin/run.sh"]
