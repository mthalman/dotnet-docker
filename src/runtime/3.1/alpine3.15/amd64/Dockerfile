ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:3.1.26-alpine3.15 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22273.3 \
    && wget -O dotnet.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-linux-musl-x64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='2cf8665c74feb5ae7917f07d8d904a0207fb6d50db41f28680be290e691bafcad7ebf5aa4f3a39e96bd700da5a5f87e05c47a4763ec973d4ff08eb1b06dc50e2' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:3.1.26-alpine3.15

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22273.3

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
