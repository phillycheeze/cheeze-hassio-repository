#this is mine
ARG BUILD_FROM
FROM $BUILD_FROM

RUN set -ex \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
       ca-certificates 

RUN set -ex \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        mopidy-tunein \
        gstreamer1.0-plugins-ugly \
        gstreamer1.0-plugins-bad \
        gstreamer1.0-libav \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


    

# Copy data for add-on

COPY run.sh /
RUN chmod a+x /run.sh
CMD [ "/run.sh" ]




