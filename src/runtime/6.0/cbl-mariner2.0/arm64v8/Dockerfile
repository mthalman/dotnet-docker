ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:6.0.6-cbl-mariner2.0-arm64v8 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=6.0.6-servicing.22267.7 \
    && curl -fSL --output dotnet.tar.gz https://dotnetbuilds.blob.core.windows.net/internal/Runtime/$dotnet_version/dotnet-runtime-6.0.6-linux-arm64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='659bf64c5f2e11f2b8cf01dc595a4280d1960bf484fc379d3a382660eea7adb6e69ace49d84522a85920ed53fa2ffb95a3b0ca0ebf63dc909b865028174ed29f' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c -


# .NET runtime image
FROM $REPO:6.0.6-cbl-mariner2.0-arm64v8

# .NET Runtime version
ENV DOTNET_VERSION=6.0.6-servicing.22267.7

COPY --from=installer /dotnet.tar.gz /dotnet.tar.gz

RUN mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz
