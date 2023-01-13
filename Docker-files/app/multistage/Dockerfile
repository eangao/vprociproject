FROM openjdk:8 AS BUILD_IMAGE

RUN apt update && apt install maven -y

# https://vsupalov.com/build-docker-image-clone-private-repo-ssh-key/
# https://stackoverflow.com/questions/69122042/pass-to-docker-build-several-build-arg-arguments
# https://stackoverflow.com/questions/42297387/docker-build-with-build-arg-with-multiple-arguments

# add credentials on build
# ARG SSH_PRIVATE_KEY
# RUN mkdir /root/.ssh/
# RUN echo "${SSH_PRIVATE_KEY}" > /root/.ssh/id_rsa

# make sure your domain is accepted
# RUN touch /root/.ssh/known_hosts
# RUN ssh-keyscan bitbucket.org >> /root/.ssh/known_hosts

# RUN git clone -b cicd-jenkins https://github.com/eangao/vprociproject.git


# https://www.baeldung.com/ops/docker-include-files-outside-build-context
COPY vprociproject/target/vprofile-v2.war /vprociproject/

RUN cd vprociproject && mvn install

FROM tomcat:8-jre11

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=BUILD_IMAGE vprociproject/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]