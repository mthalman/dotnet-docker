ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:6.0.6-alpine3.14-amd64 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=6.0.6-servicing.22267.7 \
    && wget -O dotnet.tar.gz https://dotnetbuilds.blob.core.windows.net/internal/Runtime/$dotnet_version/dotnet-runtime-6.0.6-linux-musl-x64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='c950e64d7d95e979032cf2eccbc41911380141e8fb529aca5c05f23c4eb480d0182a8f26c64305efbfe409af3ad7ed66197b10e861ac795098c653ce6b1aa41f' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:6.0.6-alpine3.14-amd64

# .NET Runtime version
ENV DOTNET_VERSION=6.0.6-servicing.22267.7

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
