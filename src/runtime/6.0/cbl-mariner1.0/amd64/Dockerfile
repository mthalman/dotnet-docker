ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:6.0.6-cbl-mariner1.0-amd64 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=6.0.6-servicing.22267.7 \
    && curl -fSL --output dotnet-host.rpm https://dotnetbuilds.blob.core.windows.net/internal/Runtime/$dotnet_version/dotnet-host-6.0.6-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='c6f5816096c5289256c0131358889d32e7dd4252c8aed52d459cf4898d3326068bb520c44088adb9d34572cc9afee858b7a0310764207967176a1b14e633fc16' \
    && echo "$dotnet_sha512  dotnet-host.rpm" | sha512sum -c - \
    \
    && curl -fSL --output dotnet-hostfxr.rpm https://dotnetbuilds.blob.core.windows.net/internal/Runtime/$dotnet_version/dotnet-hostfxr-6.0.6-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='b77395937c5c9a49338e2db29ebd0adb8ce8ba198dcd1552eee595b0257be2f2f5272ed2b715ace3147bc2d7fc1a1e6f97d7d7b82652bf5f7d97d953289f94db' \
    && echo "$dotnet_sha512  dotnet-hostfxr.rpm" | sha512sum -c - \
    \
    && curl -fSL --output dotnet-runtime.rpm https://dotnetbuilds.blob.core.windows.net/internal/Runtime/$dotnet_version/dotnet-runtime-6.0.6-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='5f03312db6f545699642d3e48e14a4383d84d821d615fa7d83c775b37ab9f3c99671c5c358583103d2576bf8f8219a72f7d35f4fc25594dd54dd8500b5265b72' \
    && echo "$dotnet_sha512  dotnet-runtime.rpm" | sha512sum -c -


# .NET runtime image
FROM $REPO:6.0.6-cbl-mariner1.0-amd64

# .NET Runtime version
ENV DOTNET_VERSION=6.0.6-servicing.22267.7

COPY --from=installer /dotnet-host.rpm /dotnet-host.rpm
COPY --from=installer /dotnet-hostfxr.rpm /dotnet-hostfxr.rpm
COPY --from=installer /dotnet-runtime.rpm /dotnet-runtime.rpm

RUN rpm --install dotnet-host.rpm dotnet-hostfxr.rpm dotnet-runtime.rpm \
    && rm dotnet-host.rpm dotnet-hostfxr.rpm dotnet-runtime.rpm
