# Use an official CUDA runtime with Ubuntu as a parent image
FROM nvidia/cuda:12.8.1-runtime-ubuntu22.04

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    ca-certificates \
    python3.11 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip3 install --no-cache-dir --upgrade pip

# Set the working directory in the container
WORKDIR /app

# Pull nightly pytorch for RTX 50x0 support
RUN pip install --no-cache-dir --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128

# Get requirements
COPY pyproject.toml .

# Install packages specified in pyproject.toml cu121, extras
RUN pip3 install --no-cache-dir .[cu121,extras]

RUN rm pyproject.toml

# Copy the current directory contents into the container
COPY . .

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Set the entry point
ENTRYPOINT ["python3"]

# Run main.py when the container launches
CMD ["main.py"]
