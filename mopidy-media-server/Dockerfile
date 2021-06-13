ARG BUILD_FROM
ARG BUILD_ARCH
FROM $BUILD_FROM

ENV LANG C.UTF-8
ENV SNAPCAST_VERSION 0.25.0
ENV ENV_BUILD_ARCH=$BUILD_ARCH

# Everything required for Mopidy and extensions
RUN set -ex \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        build-essential \
        curl \
        gcc \
        gstreamer1.0-alsa \
        gstreamer1.0-libav \
        gstreamer1.0-plugins-bad \
        gstreamer1.0-plugins-ugly \
        libxml2-dev \
        libxslt-dev \
        libz-dev \
        jq \
        python3-crypto \
        python3-dev \
        python3-pip \
    && curl -L https://apt.mopidy.com/mopidy.gpg -o /tmp/mopidy.gpg \
    && curl -L https://apt.mopidy.com/mopidy.list -o /etc/apt/sources.list.d/mopidy.list \
    && apt-key add /tmp/mopidy.gpg \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        mopidy \
        mopidy-mpd \
        mopidy-spotify \
    && pip3 install -U six oauth2client pyasn1 \
    && pip3 install \
        Mopidy-Iris 

# Snapcast stuff
RUN set -ex \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        libasound2-dev libvorbisidec-dev libvorbis-dev libflac-dev alsa-utils libavahi-client-dev avahi-daemon fakeroot \
    && cd /tmp/ \
    && curl -L https://github.com/badaix/snapcast/releases/download/v${SNAPCAST_VERSION}/snapserver_${SNAPCAST_VERSION}-1_${BUILD_ARCH}.deb -o snapserver.deb \
    && DEBIAN_FRONTEND=noninteractive apt install -y snapserver.deb \
    && apt-get clean \
    && apt-get purge --auto-remote -y \
        gcc \
        curl \
        buildessential \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* ~/.cache

# Copy data for add-on
COPY mopidy.conf /var/lib/mopidy/.config/mopidy/mopidy.conf

COPY run.sh /
RUN chmod a+x /run.sh

EXPOSE 6600 6680 1704 1705
CMD [ "/run.sh" ]




