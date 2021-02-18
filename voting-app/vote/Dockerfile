# Using official python runtime base image
FROM python:3.9-slim

# add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Set the application directory
WORKDIR /app

ENV PATH="/app/.local/bin/:${PATH}"

RUN useradd -u 1001 -r -g 0 -d /app -s /sbin/nologin \
    -c "Default Application User" default && \
    chown -R 1001:0 /app && \
    chmod -R g+rw /app

USER 1001

# Install our requirements.txt
COPY src/requirements.txt /app/requirements.txt
RUN pip install -r requirements.txt

# Copy our code from the current folder to /app inside the container
COPY src/. .

# Make port 80 available for links and/or publish
EXPOSE 8080

# Define our command to be run when launching the container
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:8080", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0"]
