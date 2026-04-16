# Use the official Python image as the base image
FROM python:3.12.7-alpine

# Set the working directory to /app
WORKDIR /app

# Set the build argument for the app version number
ARG APP_VERSION=0.1.0

# Upgrade system packages to patch known CVEs
RUN apk upgrade --no-cache

# Create a non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy the requirements file into the container
COPY requirements.txt .

# Install the dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code into the container
COPY . .

# Prevent Python from writing .pyc files to disc
ENV PYTHONDONTWRITEBYTECODE=1

# Expose port 5001 for the FastAPI application
EXPOSE 5001

# Set the environment variable for the app version number
ENV APP_VERSION=$APP_VERSION

# Add a health check
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:5001/health || exit 1

# Switch to non-root user
USER appuser

# Start the FastAPI application
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5001"]