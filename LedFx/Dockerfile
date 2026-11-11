
# BASE HOME ASSISTANT
FROM ghcr.io/home-assistant/base-python:latest

# DEPENDENCIES
RUN apk update && apk upgrade --no-cache && apk add --no-cache \
    alsa-plugins-pulse \
    g++ \
    gcc \
    git \
    linux-headers \
    make \
    nginx \
    portaudio-dev \
    python3-dev \
    eudev-dev

# PYTHON BUILD TOOLS
RUN /usr/local/bin/python -m pip install --no-cache-dir --upgrade pip setuptools wheel

# NUMPY AND MESON
RUN /usr/local/bin/python -m pip install --no-cache-dir \
    "numpy>=2.0,<3" \
    meson-python \
    "meson>=1.9.0" \
    ninja

# VERIFY NUMPY
RUN /usr/local/bin/python -c "import numpy; print('NumPy:', numpy.__version__, numpy.__file__)"

# BUILD AUBIO SEPARATELY
RUN /usr/local/bin/python -m pip install --no-cache-dir --no-build-isolation aubio-ledfx==0.4.12

# INSTALL LEDFX
RUN /usr/local/bin/python -m pip install --no-cache-dir ledfx==2.2.1

# SENDSPIN FIX
RUN wget -qO "$(/usr/local/bin/python -c 'import ledfx,os;print(os.path.dirname(ledfx.__file__))')/sendspin/stream.py" \
    https://raw.githubusercontent.com/LedFx/LedFx/d0423a6bd6c4044224515b35149a0e91a865cfe1/ledfx/sendspin/stream.py

# FILES
COPY rootfs /

# PERMISSIONS
RUN chmod a+x \
    "/etc/s6-overlay/s6-rc.d/init/run" \
    "/etc/s6-overlay/s6-rc.d/ledfx/run" \
    "/etc/s6-overlay/s6-rc.d/nginx/run"
