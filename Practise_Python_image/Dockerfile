# Example #2 — Python API Using Gunicorn

# “In another role, we built a Python Flask API. 
# The build stage installed all dependencies and compiled C extensions, while the runtime stage used a minimal Python image with only the required wheels copied in. 
# This ensured faster deployments and reduced vulnerabilities from unused tools.”

# Stage 1 - Build dependencies
FROM python:3.11 as builder
WORKDIR /app
COPY requirements.txt .
RUN pip wheel -r requirements.txt --wheel-dir /wheels

# Stage 2 - Minimal runtime
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /wheels /wheels
RUN pip install --no-index --find-links=/wheels -r /wheels/requirements.txt
COPY . .
CMD ["gunicorn", "app:app", "--bind", "0.0.0.0:8000"]
