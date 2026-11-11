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

# BUILD TOOLS + NUMPY (les guillemets sont obligatoires : sans eux, ">" est une redirection shell)
RUN python3 -m pip install --no-cache-dir --upgrade pip setuptools wheel \
    "numpy>=2.0" meson-python "meson>=1.9.0" ninja

# DIAGNOSTIC : si numpy est inutilisable, le build s'arrête ici avec la vraie erreur
RUN python3 -c "import sys, numpy; print(sys.version); print(numpy.__version__, numpy.__file__)"

# APP (sans build isolation : aubio-ledfx utilise le numpy installé ci-dessus)
RUN python3 -m pip install --no-cache-dir --no-build-isolation \
    aubio-ledfx==0.4.12 ledfx==2.2.1

# SENDSPIN IS BROKEN IN CURRENT RELEASE, APPLYING FIX FROM MAINBRANCH
RUN wget -qO "$(python3 -c 'import ledfx,os;print(os.path.dirname(ledfx.__file__))')/sendspin/stream.py" \
    https://raw.githubusercontent.com/LedFx/LedFx/d0423a6bd6c4044224515b35149a0e91a865cfe1/ledfx/sendspin/stream.py

# FILES
COPY rootfs /

# PERMISSIONS
RUN chmod a+x \
    "/etc/s6-overlay/s6-rc.d/init/run" \
    "/etc/s6-overlay/s6-rc.d/ledfx/run" \
    "/etc/s6-overlay/s6-rc.d/nginx/run"
