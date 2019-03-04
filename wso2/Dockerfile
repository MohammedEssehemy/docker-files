FROM wso2/wso2is:5.7.0

LABEL maintainer="Mohammed Essehemy <mohammedessehemy@gmail.com>"

USER root

# allow unprivileged user to run java on ports less than 1024
RUN apt update && apt install -y libcap2-bin && \
    setcap cap_net_bind_service=+ep  /opt/java/openjdk/bin/java && \
    echo "/opt/java/openjdk/lib/amd64/jli" > /etc/ld.so.conf.d/java.conf && \
    ldconfig -v && \
    sed -i 's:u2f-api.js:/portal/gadgets/user_profile/js/u2f-api.js:g' /home/wso2carbon/wso2is-5.7.0/repository/deployment/server/jaggeryapps/portal/gadgets/user_profile/js/gadget.js

USER 802
