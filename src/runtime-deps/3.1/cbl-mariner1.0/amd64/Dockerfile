# Installer image
FROM mcr.microsoft.com/cbl-mariner/base/core:1.0 AS installer

ARG SAS_QUERY_STRING

RUN dotnet_version=3.1.26-servicing.22273.3 \
    && curl -fSL --output dotnet-runtime-deps.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-deps-3.1.26-cm.1-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='b9b758318a674c91ef95970c745497acf7c23d8d910df09532ff6e0956d28035c29592b0735a0664b670a052636e4ff1f4e9c86829b6dbe016c7814b105614e7' \
    && echo "$dotnet_sha512  dotnet-runtime-deps.rpm" | sha512sum -c -

FROM mcr.microsoft.com/cbl-mariner/base/core:1.0

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
