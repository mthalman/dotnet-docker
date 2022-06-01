ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:6.0.6-alpine3.15-arm32v7 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=6.0.6-servicing.22267.7 \
    && wget -O dotnet.tar.gz https://dotnetbuilds.blob.core.windows.net/internal/Runtime/$dotnet_version/dotnet-runtime-6.0.6-linux-musl-arm.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='8bc37b9a22c6ffb944d511f757d5ba43359ffe9579f09b5a618a70004b615c87310b31f546ac3e76b045244a65177e7546b7715536ea56e831299e52de6c91d5' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:6.0.6-alpine3.15-arm32v7

# .NET Runtime version
ENV DOTNET_VERSION=6.0.6-servicing.22267.7

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
