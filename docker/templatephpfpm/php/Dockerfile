# This Dockerfile uses the latest version of the Bitnami PHP-FPM Docker image
# FROM bitnami/php-fpm:8.2.6    - vulnerable
# FROM php:8.2.5-zts-buster     - vulnerable
# FROM bitnami/php-fpm:latest   - vulnerable
# FROM php:8.2.6-fpm            - vulnerable
# FROM php:8.2.7-fpm            - vulnerable
# FROM php:8.2.9-fpm-bookworm   - vulnerable
FROM php:8-fpm

# refactor : SonarQube Security Hotspots Compliance fix (run as nonroot)
RUN addgroup --gid 1000 nongroup
# RUN adduser --quiet --disabled-password --uid 1000 --gid 1000 --shell /bin/bash nonroot
RUN adduser --quiet --disabled-password --gecos "" --uid 1000 --gid 1000 --shell /bin/bash nonroot

RUN apt-get update
RUN apt-get --no-install-recommends install wget -y

# Enable Datadog APM/Profiling
# refactor : SonarQube Security Hotspots Compliance fix
RUN wget --secure-protocol=TLSv1_2 -q https://github.com/DataDog/dd-trace-php/releases/latest/download/datadog-setup.php -O datadog-setup.php
RUN php datadog-setup.php --php-bin=all --enable-profiling
# RUN php datadog-setup.php --php-bin=all --enable-appsec --enable-profiling

# Custom Config updates
COPY ./php/config/custom.ini /opt/bitnami/php/etc/conf.d/custom.ini

# Copy app's source code to the /app directory
COPY app-code /app

# The application's directory will be the working directory
WORKDIR /app

# refactor : SonarQube Security Hotspots Compliance fix
USER nonroot
