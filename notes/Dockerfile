# Use the Python 3.9 version of Alpine Linux as the base image for this Docker container
FROM python:3.9-alpine3.13

# Set a label for this Docker image to identify the maintainer
LABEL maintainer="doganaktar"

# Set an environment variable to ensure that Python's output is not buffered, allowing logs to be seen immediately
ENV PYTHONUNBUFFERED 1 

# Copy the requirements.txt file from the local machine to a temporary location in the container
COPY ./requirements.txt /tmp/requirements.txt
COPY ./requirements.dev.txt /tmp/requirements.dev.txt

# Copy all of the application's code from the local machine into the container, and place it in the /app directory
COPY ./app /app

# Set the working directory for the container to be /app, which contains the application's code
WORKDIR /app

# Expose port 8000 on the container to allow communication with the application running inside the container
EXPOSE 8000

#runs a command in alpine image, cok layer olmamasi icin tek run commandinde yaptik

# Create a virtual environment in /py directory and activate it
# Use the Python module venv to create a new virtual environment called "/py" in the container.
ARG DEV=false
RUN python -m venv /py && \ 
    # Upgrade pip within the virtual environment
    /py/bin/pip install --upgrade pip && \
    # Install all packages listed in /tmp/requirements.txt file into the virtual environment
    /py/bin/pip install -r /tmp/requirements.txt && \
    /py/bin/pip install -r /tmp/requirements.txt && \
    #[ x ] should be like this [x] is wrong
    if [ $DEV = "true" ]; \ 
        then /py/bin/pip install -r /tmp/requirements.dev.txt ; \
    fi && \
    # Remove /tmp directory and its contents, which is no longer needed after installation
    rm -rf /tmp && \
    # Create a new user account called 'django-user' with disabled password and without creating a home directory This can help keep the container image small and secure, as it reduces the attack surface area for potential security vulnerabilities.
    adduser \
        --disabled-password \
        --no-create-home \
        django-user

ENV PATH="/py/bin:$PATH"

USER django-user
