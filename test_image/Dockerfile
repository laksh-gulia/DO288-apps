FROM registry.access.redhat.com/ubi8/ubi:8.0

# Labels consumed by OpenShift
LABEL io.k8s.description="A basic Apache HTTP Server child image, uses ONBUILD" \
      io.k8s.display-name="Apache HTTP Server" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="apache, httpd"

# DocumentRoot for Apache
ENV DOCROOT=/var/www/html 

RUN   yum install -y --nodocs --disableplugin=subscription-manager httpd && \
      yum clean all --disableplugin=subscription-manager -y && \
      echo "Hello from the httpd-parent container!" > ${DOCROOT}/index.html

# Allows child images to inject their own content into DocumentRoot
ONBUILD COPY src/ ${DOCROOT}/

EXPOSE 8080

# This stuff is needed to ensure a clean start
RUN sed -i "s/Listen 80/Listen 8080/g" /etc/httpd/conf/httpd.conf && \
    chgrp -R 0 /var/log/httpd /var/run/httpd && \
    chmod -R g=u /var/log/httpd /var/run/httpd 

# Run as the root user
USER root

CMD /usr/sbin/httpd -DFOREGROUND

