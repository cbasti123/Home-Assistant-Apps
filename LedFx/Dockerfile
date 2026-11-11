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

# PIP UPGRADE
RUN python3 -m pip install --no-cache-dir --upgrade pip setuptools wheel

# 1. Télécharger les sources d'aubio-ledfx
RUN git clone https://github.com/LedFx/aubio-ledfx.git /tmp/aubio-ledfx

# 2. Injecter numpy dans le pyproject.toml pour que pip l'installe dans son isolation
RUN sed -i 's/requires = \[/requires = \["numpy>=2.0", /g' /tmp/aubio-ledfx/pyproject.toml

# 3. Installer aubio-ledfx localement (cette fois, numpy sera présent dans l'isolation)
RUN python3 -m pip install --no-cache-dir /tmp/aubio-ledfx

# 4. Installer LedFx ensuite
RUN python3 -m pip install --no-cache-dir ledfx==2.2.1

# SENDSPIN FIX
RUN wget -qO "$(python3 -c 'import ledfx,os;print(os.path.dirname(ledfx.__file__))')/sendspin/stream.py" \
    https://raw.githubusercontent.com/LedFx/LedFx/d0423a6bd6c4044224515b35149a0e91a865cfe1/ledfx/sendspin/stream.py

# FILES
COPY rootfs /

# PERMISSIONS
RUN chmod a+x \
    "/etc/s6-overlay/s6-rc.d/init/run" \
    "/etc/s6-overlay/s6-rc.d/ledfx/run" \
    "/etc/s6-overlay/s6-rc.d/nginx/run"
