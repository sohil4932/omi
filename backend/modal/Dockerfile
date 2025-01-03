FROM python:3.11 AS builder

ENV PATH="/opt/venv/bin:$PATH"
RUN python -m venv /opt/venv

COPY backend/requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

FROM python:3.11-slim

WORKDIR /app
ENV PATH="/usr/local/nvidia/bin:/usr/local/cuda/bin:/opt/venv/bin:$PATH"
ENV LD_LIBRARY_PATH="/usr/local/nvidia/lib:/usr/local/nvidia/lib64"

RUN apt-get update && apt-get -y install build-essential ffmpeg curl unzip wget software-properties-common

COPY --from=builder /opt/venv /opt/venv
COPY backend/database /app/database
COPY backend/utils /app/utils
COPY backend/modal/ .

EXPOSE 8080
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8080"]
