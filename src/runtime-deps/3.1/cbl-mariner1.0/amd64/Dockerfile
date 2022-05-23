# Installer image
FROM mcr.microsoft.com/cbl-mariner/base/core:1.0 AS installer

RUN dotnet_version=3.1.26-servicing.22261.2 \
    && curl -fSL --output dotnet-runtime-deps.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-deps-$dotnet_version-cm.1-x64.rpmSAS_QUERY_STRING \
    && dotnet_sha512='3e480745451cea4a60bfc253ac10cec6c6005f61854a100782bed11bd9d5899719e552271a357d77301ca1152f30855cd7f099bba89db1061cad777e576bc522' \
    && echo "$dotnet_sha512  dotnet-runtime-deps.rpm" | sha512sum -c -

RUN tdnf install -y \
        ca-certificates \
        \
        # .NET Core dependencies
        glibc \
        icu \
        krb5 \
        libgcc \
        libstdc++ \
        openssl-libs \
        zlib \
    && tdnf clean all

COPY --from=installer /dotnet-runtime-deps.rpm /dotnet-runtime-deps.rpm

# Install dotnet-runtime-deps package
RUN rpm --install dotnet-runtime-deps.rpm \
    && rm dotnet-runtime-deps.rpm

ENV \
    # Configure web servers to bind to port 80 when present
    ASPNETCORE_URLS=http://+:80 \
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true
