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

# PYTHON BUILD TOOLS, NUMPY & MESON
RUN /usr/local/bin/python -m pip install --no-cache-dir --upgrade pip setuptools wheel "numpy>=2.0,<3" meson-python "meson>=1.9.0" ninja

# INSTALL AUBIO AND LEDFX (avec isolation standard pour que meson-python trouve numpy dans l'environnement de build)
RUN /usr/local/bin/python -m pip install --no-cache-dir aubio-ledfx==0.4.12 ledfx==2.2.1

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
