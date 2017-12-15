FROM debian:jessie

# run and get ssl-bunp enabled squid3 deb package on /dist

RUN echo '\n\
deb-src http://deb.debian.org/debian jessie main\n\
deb-src http://deb.debian.org/debian jessie-updates main\n\
deb-src http://security.debian.org jessie/updates main\n'\
>> /etc/apt/sources.list

RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends \
    build-essential \
    libssl-dev libcrypto++-dev \
    && apt-get source -y squid3 \
    && apt-get build-dep -y squid3 \
    && cd squid3-* \
    && sed -i -e 's/DEB_CONFIGURE_EXTRA_FLAGS := /DEB_CONFIGURE_EXTRA_FLAGS := --enable-ssl --enable-ssl-crtd /' debian/rules \
    && dpkg-buildpackage -us -uc -b -j$(nproc) \
    && cd .. \
    && mkdir /dist \
    && mv /*.deb /*.changes /dist \
    && rm -rf squid3-* squid3_* \
    && apt-get purge -y --auto-remove build-essential libssl-dev libcrypto++-dev $(apt-cache showsrc squid3 | sed -e '/Build-Depends/!d;s/Build-Depends: \|,\|([^)]*),*\|\[[^]]*\]//g') \
	&& rm -rf /var/lib/apt/lists/*
