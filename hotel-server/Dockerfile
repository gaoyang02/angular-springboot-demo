FROM java:8
ENV TZ=Asia/Shanghai
VOLUME /tmp
EXPOSE 8080
ENV JAVA_OPTS=$JAVA_OPTS
WORKDIR /home/apps/
ADD target/cms-1.0-SNAPSHOT.jar .
ENTRYPOINT ["sh","-c","java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom,java -Djava.awt.headless=true,-Dfastjson.parser.safeMode=true,-agentlib:jdwp=transport=dt_socket,address=0:8000,server=y,suspend=n -jar /home/apps/cms-1.0-SNAPSHOT.jar"]
