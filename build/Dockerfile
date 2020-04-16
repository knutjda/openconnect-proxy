FROM ubuntu:devel

RUN apt-get update -y && apt-get install -y apt-utils
RUN apt-get install -y openconnect tinyproxy openssl
RUN apt-get install -y wget \
    && apt-get -y install build-essential \
    && wget https://github.com/rofl0r/microsocks/archive/v1.0.1.tar.gz \
    && tar -xzvf v1.0.1.tar.gz \
    && cd microsocks-1.0.1 \
    && make \
    && make install \
    # always add the docker DNS server
    && grep -qxF 'nameserver 127.0.0.11' /etc/resolv.conf || echo 'nameserver 127.0.0.11' >> /etc/resolv.conf


# Use an up-to-date version of vpnc-script
# https://www.infradead.org/openconnect/vpnc-script.html
COPY vpnc-script-org /usr/share/vpnc-scripts/vpnc-script-org
RUN chmod 755 /usr/share/vpnc-scripts/vpnc-script-org
COPY vpnc-script /usr/share/vpnc-scripts/vpnc-script
RUN chmod 755 /usr/share/vpnc-scripts/vpnc-script

COPY tinyproxy.conf /etc/tinyproxy.conf

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

EXPOSE 8888
EXPOSE 8889

ENTRYPOINT ["/entrypoint.sh"]
