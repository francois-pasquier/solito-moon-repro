#### BASE
FROM node:21.7.1-alpine3.19 AS base

ENV MOON_TOOLCHAIN_FORCE_GLOBALS=1

WORKDIR /app

# Install moon binary
RUN npm install -g @moonrepo/cli

#### WORKSPACE
FROM base AS workspace
WORKDIR /app

# Copy entire repository and scaffold
COPY . .

RUN moon docker scaffold next

#### BUILD
FROM base AS build
WORKDIR /app

# Copy workspace skeleton
COPY --from=workspace /app/.moon/docker/workspace .

# Install toolchain and dependencies
RUN moon docker setup

# Copy source files
COPY --from=workspace /app/.moon/docker/sources .

# Run something
RUN moon run next:build --log trace

# Prune workspace
RUN moon docker prune

EXPOSE 8080
ENV PORT 8080

