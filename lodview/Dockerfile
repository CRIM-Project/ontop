#
# based on https://github.com/LodLive/LodView/blob/master/Dockerfile, modified to
# - pick a recent commit (2021-11-08, after v1.2.1) that compiles correctly (head may be broken, e.g., missing Dotenv dependency)
# - produce and deploy an unpacked web app (vs. a war), so to override configuration files via Docker volumes
# - reduce logging verbosity
#

FROM maven:3.8.7-eclipse-temurin-11 AS builder
RUN git clone https://github.com/LodLive/LodView.git ; \
    cd LodView ; \
    git checkout 7ce43c3 ; \
    mvn compile org.apache.maven.plugins:maven-war-plugin:3.3.2:exploded ; \
    rm target/lodview/WEB-INF/lib/slf4j-log4j12-1.7.6.jar

FROM tomcat:9.0.71-jre11-temurin
ENV CATALINA_OPTS="-XX:+UseSerialGC"
COPY --from=builder /LodView/target/lodview /usr/local/tomcat/webapps/ROOT
RUN sed -i 's/port="8080"/port="8888"/' /usr/local/tomcat/conf/server.xml && \
    echo -e \
    "handlers = java.util.logging.ConsoleHandler\n" \
    ".handlers = java.util.logging.ConsoleHandler\n" \
    ".level = WARNING\n" \
    "java.util.logging.ConsoleHandler.level = WARNING\n" \
    "java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter\n" \
    "java.util.logging.ConsoleHandler.encoding = UTF-8\n" > /usr/local/tomcat/conf/logging.properties
CMD ["catalina.sh", "run"]
EXPOSE 8888 8009
