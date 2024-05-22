FROM ubuntu:focal
WORKDIR /opt/erpnet-fp
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        curl \
        ca-certificates \
        \
        # .NET dependencies
        libc6 \
        libgcc1 \
        libgssapi-krb5-2 \
        libicu66 \
        libssl1.1 \
        libstdc++6 \
        zlib1g \
        unzip \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin -Channel 8.0 -Runtime dotnet -InstallDir /usr/share/dotnet \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

COPY Output/ /usr/src/
RUN mkdir -p /opt/erpnet-fp
RUN if [ -z "${TARGETARCH}" ]; then \
      TARGETARCH="$(dpkg --print-architecture)"; \
    fi; \
    case ${TARGETARCH} in \
      "amd64") unzip /usr/src/linux-x64.zip -d /opt/erpnet-fp  ;; \
      "arm64") unzip /usr/src/linux-arm64.zip -d /opt/erpnet-fp  ;; \
      "armhf") unzip /usr/src/linux-arm.zip -d /opt/erpnet-fp  ;; \
    esac

ENTRYPOINT ["dotnet", "ErpNet.FP.Server.dll"]
