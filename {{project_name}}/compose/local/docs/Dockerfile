# Base Python image
FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim AS builder

# Build arguments
ARG APP_HOME=/app
ARG USER_NAME=app
ARG USER_UID=1000
ARG USER_GID=1000



# Build stage
WORKDIR ${APP_HOME}

# Install build dependencies
RUN apt-get update && apt-get install --no-install-recommends -y \
    build-essential \
    libpq-dev \
    gettext \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY pyproject.toml uv.lock ./
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --no-install-project

# Runtime stage
FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim

ARG APP_HOME=/app
ARG USER_NAME=app
ARG USER_UID=1000
ARG USER_GID=1000

# Environment configuration
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    UV_COMPILE_BYTECODE=1 \
    UV_LINK_MODE=copy \
    PATH="/app/.venv/bin:$PATH"

# Install runtime dependencies
RUN apt-get update && apt-get install --no-install-recommends -y \
    make \
    libpq-dev \
    gettext \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create non-privileged user
RUN groupadd --gid ${USER_GID} ${USER_NAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USER_NAME}

# Setup application
WORKDIR ${APP_HOME}
COPY --from=builder --chown=${USER_NAME}:${USER_NAME} ${APP_HOME} ${APP_HOME}
COPY --chown=${USER_NAME}:${USER_NAME} ./compose/local/docs/start /start-docs
RUN sed -i 's/\r$//g' /start-docs && chmod +x /start-docs

# Switch to non-root user
USER ${USER_NAME}

WORKDIR /docs