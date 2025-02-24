FROM python:3.10-slim

# Add build argument for additional packages
ARG ADDITIONAL_PYTHON_PACKAGES=""

# Install system dependencies for OpenCV
RUN apt-get update && apt-get install -y \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender-dev \
    libgl1-mesa-glx \
    ffmpeg \
    jq \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Python packages
# Note: torch will detect the correct version for the platform
RUN pip install --no-cache-dir \
    numpy \
    "torch>=2.2.0" \
    opencv-python \
    PytorchWildlife \
    "tqdm>=4.65.0" \
    ${ADDITIONAL_PYTHON_PACKAGES}

# Preload models
RUN echo "Downloading models... Can take a while..." && python -c "from PytorchWildlife.models import detection; detection.MegaDetectorV6(device='cpu', pretrained=True, version='MDV6-yolov9-c')"

WORKDIR /app
COPY app.sh detect.py ./
RUN chmod +x app.sh

CMD ["./app.sh"]
