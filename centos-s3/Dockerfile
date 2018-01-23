FROM maven:3.5.2-jdk-8-alpine as s3-blobstore
WORKDIR /tmp/nexus-blobstore-s3
COPY ../nexus-blobstore-s3/ .
RUN mvn clean install && \
    PROJ_VERSION=$(mvn -q \
    -Dexec.executable="echo" \
    -Dexec.args='${project.version}' \
    --non-recursive \
    org.codehaus.mojo:exec-maven-plugin:1.3.1:exec) && \
    echo "$PROJ_VERSION" > version

FROM sonatype/nexus3:3.7.1

## Install Blobstore plugin ##
USER root
COPY --from=s3-blobstore /tmp/nexus-blobstore-s3/version /tmp/
COPY --from=s3-blobstore /tmp/nexus-blobstore-s3/target/nexus-blobstore-s3-*.jar /tmp/nexus-blobstore-s3/
RUN S3_BLOBSTORE_VERSION=$(cat /tmp/version) && \
    mkdir -p ${NEXUS_HOME}/system/org/sonatype/nexus/nexus-blobstore-s3/${S3_BLOBSTORE_VERSION}/ && \
    mv /tmp/nexus-blobstore-s3/nexus-blobstore-s3-*.jar ${NEXUS_HOME}/system/org/sonatype/nexus/nexus-blobstore-s3/${S3_BLOBSTORE_VERSION}/ && \
    sed -i.bak -e "/nexus-blobstore-file/a\\"$'\n'"<bundle>mvn:org.sonatype.nexus/nexus-blobstore-s3/${S3_BLOBSTORE_VERSION}</bundle>" ${NEXUS_HOME}/system/org/sonatype/nexus/assemblies/nexus-base-feature/*/nexus-base-feature-*-features.xml && \
    sed -i.bak -e "/nexus-blobstore-file/a\\"$'\n'"<bundle>mvn:org.sonatype.nexus/nexus-blobstore-s3/${S3_BLOBSTORE_VERSION}</bundle>" ${NEXUS_HOME}/system/org/sonatype/nexus/assemblies/nexus-core-feature/*/nexus-core-feature-*-features.xml

USER nexus
