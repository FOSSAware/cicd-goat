FROM alpine:3.15.3
LABEL org.opencontainers.image.vendor="Cider Security" \
    org.opencontainers.image.title="CI/CD Goat - Lighttpd" \
    org.opencontainers.image.description="Deliberately vulnerable CI/CD environment." \
    org.opencontainers.image.url="https://hub.docker.com/r/cidersecurity/goat-lighttpd" \
    org.opencontainers.image.source="https://github.com/cider-security-research/cicd-goat" \
    org.opencontainers.image.licenses="GPL-3.0" \
    org.opencontainers.image.version="${VERSION}" \
    org.opencontainers.image.revision="${COMMIT_SHA}"

RUN apk add --no-cache \
        openssh \
    && ssh-keygen -A \
    && mkdir /root/.ssh \
    && chmod 0700 /root/.ssh
COPY authorized_keys /root/.ssh/
COPY entrypoint.sh /usr/sbin
COPY sshd_config /etc/ssh/
RUN chmod 600 /root/.ssh/authorized_keys && \
    chmod 755 /usr/sbin/entrypoint.sh && \
    apk update \
    && apk add lighttpd \
    && rm -rf /var/cache/apk/* \
    && passwd -u root
COPY reportcov.sh /var/www/localhost/htdocs

EXPOSE 22
EXPOSE 80

CMD ["sh", "/usr/sbin/entrypoint.sh"]
