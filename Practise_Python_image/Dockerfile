
#I use a multi-stage Docker build to separate dependency preparation from application runtime. 
#The builder stage prepares Python wheels, and the slim runtime stage installs those wheels and 
#copies the application code.
#This reduces the final image size and avoids requiring network access to install dependencies 
#during the final stage."

# Stage 1: Builder stage to prepare Python dependencies
FROM python:3.11 AS builder
# Uses Python 3.11 as the base image and names this stage "builder"

WORKDIR /app
# Sets /app as the working directory inside the container

COPY requirements.txt .
# Copies the dependency file from the host into /app

RUN pip wheel -r requirements.txt --wheel-dir /wheels
# Downloads/builds Python dependency wheels and stores them in /wheels
# This helps install dependencies in the final image without rebuilding them

# Stage 2: Final runtime image
FROM python:3.11-slim
# Uses a smaller Python image to reduce the final image size

WORKDIR /app
# Sets /app as the working directory

COPY --from=builder /wheels /wheels
# Copies the prepared dependency wheels from the builder stage

COPY --from=builder /app/requirements.txt /app/requirements.txt
# Copies the requirements file needed for dependency installation

RUN pip install --no-cache-dir --no-index \
    --find-links=/wheels -r requirements.txt
# Installs dependencies from local wheels without accessing the internet
# --no-cache-dir avoids storing pip's download cache in the image

COPY . .
# Copies the application source code into the container

EXPOSE 8000
# Documents that the application listens on port 8000

CMD ["gunicorn", "app:app", "--bind", "0.0.0.0:8000"]
# Starts the Flask application using Gunicorn on port 8000
# app:app means app.py file and its Flask application object named "app"

