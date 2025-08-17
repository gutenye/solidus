FROM ruby:3.3-slim

RUN apt-get update -y && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    build-essential git curl \
    libpq-dev postgresql-client \
    nodejs \
    libvips \
		libyaml-dev \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /app
ENV BUNDLE_PATH=/bundle BUNDLE_JOBS=4 BUNDLE_RETRY=3

# Create a bare Rails app, then install Solidus
# (if you already have a Rails app, replace this with COPY Gemfile etc.)
RUN gem install rails -v "~>7.1" \
 && rails new . -T --database=postgresql

# Install Solidus (adds gem to Gemfile + runs generator)  
RUN bundle add solidus \
 && bin/rails generate solidus:install --auto-accept

# COPY docker/entrypoint.sh /usr/local/bin/entrypoint.sh
# RUN chmod +x /usr/local/bin/entrypoint.sh

EXPOSE 3000
# ENTRYPOINT ["entrypoint.sh"]
CMD ["bin/rails", "server", "-b", "0.0.0.0", "-p", "3000"]