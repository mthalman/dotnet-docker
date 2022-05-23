ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:3.1.26-alpine3.14-arm64v8 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22261.2 \
    && wget -O dotnet.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-linux-musl-arm64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='6061f56c9834b12f35b2bf356086d90bab935c1b630b9de2fcf99a1865592ed7be2b7e18d9f596e408b5f3f3e3c2bdafe2e7ca69981fe0dfc44a6babda813777' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:3.1.26-alpine3.14-arm64v8

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22261.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
