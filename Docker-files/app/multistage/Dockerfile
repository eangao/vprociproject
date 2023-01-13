FROM openjdk:8 AS BUILD_IMAGE
RUN apt update && apt install maven -y

#SSH_PRIVATE_KEY from jenkins server
RUN mkdir /root/.ssh/
RUN echo "${SSH_PRIVATE_KEY}" > /root/.ssh/id_rsa
RUN git clone -b cicd-jenkins git@github.com:eangao/vprociproject.git
RUN cd vprociproject && mvn install

FROM tomcat:8-jre11

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=BUILD_IMAGE vprociproject/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
