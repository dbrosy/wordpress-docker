FROM wordpress:7.0.0-php8.3-apache AS wordpress

# ---------------------------------------
# Custom Config
# ---------------------------------------

# custom php config         - skipped
# custom .htaccess config   - skipped

# custom redis config
RUN pecl install redis && \
	docker-php-ext-enable redis
# custom redis config


# custom wp-cli
ENV WORDPRESS_CLI_GPG_KEY=63AF7AA15067C05616FDDD88A3A2E8F226F0BC06

ENV WORDPRESS_CLI_VERSION=2.12.0
ENV WORDPRESS_CLI_SHA512=be928f6b8ca1e8dfb9d2f4b75a13aa4aee0896f8a9a0a1c45cd5d2c98605e6172e6d014dda2e27f88c98befc16c040cbb2bd1bfa121510ea5cdf5f6a30fe8832

# Install wp-cli pre-requisites
RUN set -eux; \
	\
	apt-get update; \
	apt-get install -y --no-install-recommends \
		sudo \
		less \
		mariadb-client \
	; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*

# Add WP-CLI
RUN curl -fsSL "https://github.com/wp-cli/wp-cli/releases/download/v${WORDPRESS_CLI_VERSION}/wp-cli-${WORDPRESS_CLI_VERSION}.phar" > /usr/local/bin/wp-cli.phar
COPY wp_su.sh /usr/local/bin/wp
RUN chmod +x /usr/local/bin/wp-cli.phar /usr/local/bin/wp

# Add WP-CLI Autocomplete
RUN curl -fsSL "https://raw.githubusercontent.com/wp-cli/wp-cli/v${WORDPRESS_CLI_VERSION}/utils/wp-completion.bash" > ~/wp-completion.bash
RUN echo "source ~/wp-completion.bash" >> ~/.bashrc

# Fix cache permission issue adding .wp-cli directory
RUN mkdir /var/www/.wp-cli \
    && chown www-data:www-data /var/www/.wp-cli

# custom wp-cli