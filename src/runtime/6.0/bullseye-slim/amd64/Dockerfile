ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:bullseye-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=6.0.6-servicing.22267.7 \
    && curl -fSL --output dotnet.tar.gz https://dotnetbuilds.blob.core.windows.net/internal/Runtime/$dotnet_version/dotnet-runtime-6.0.6-linux-x64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='4fe090f934f0ba4e64a63dfccbac97d49b19a913f2a7d73abe85efd604ee5577cefd65d6e0dc02086e9fa28be4ce2bbaecb33ea70d022714138ed54deea58c72' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:6.0.6-bullseye-slim-amd64

# .NET Runtime version
ENV DOTNET_VERSION=6.0.6-servicing.22267.7

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
