FROM nginx:1.13.8

RUN useradd -m -s /bin/bash -u 1000 styleb && \
    sed -i 's/user\ \ nginx\;/user\ \ styleb\;/g' /etc/nginx/nginx.conf

ADD ./default.conf /etc/nginx/conf.d/default.conf
WORKDIR /var/www
