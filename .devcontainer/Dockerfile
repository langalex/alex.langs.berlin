ARG VARIANT="4.0"
FROM mcr.microsoft.com/devcontainers/ruby:${VARIANT}

WORKDIR /workspace

COPY Gemfile* /workspace/
RUN bundle install
