ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:3.1.26-cbl-mariner1.0 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22273.3 \
    && curl -fSL --output dotnet-host.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-host-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='99e2ef831880349fd6557be8d450b1530425adbceeb857ef8fda2d89418b22537ff41ae04f39231b0f1eb0b8060a5c4e6b6cbc43170da97e29c142806a19643d' \
    && echo "$dotnet_sha512  dotnet-host.rpm" | sha512sum -c - \
    \
    && curl -fSL --output dotnet-hostfxr.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-hostfxr-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='98309be8f7ea041a4453fdc441fbdf2c7e14ca89013f4d823328274c70c4eb861484229ab86d632623348f4d6ec8cb00cfe9f3194e73228c1c30424031ad7ac5' \
    && echo "$dotnet_sha512  dotnet-hostfxr.rpm" | sha512sum -c - \
    \
    && curl -fSL --output dotnet-runtime.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='b1df9cd5bd47d8e67169b7520284ab1ef50462c2cd71510594ae7c4ed9281a0541b637ccdace46523a2ae0197e5e4c46e7fdea4e74b91a0e9a9a65e113287939' \
    && echo "$dotnet_sha512  dotnet-runtime.rpm" | sha512sum -c -


# .NET runtime image
FROM $REPO:3.1.26-cbl-mariner1.0

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22273.3

COPY --from=installer /dotnet-host.rpm /dotnet-host.rpm
COPY --from=installer /dotnet-hostfxr.rpm /dotnet-hostfxr.rpm
COPY --from=installer /dotnet-runtime.rpm /dotnet-runtime.rpm

RUN rpm --install dotnet-host.rpm dotnet-hostfxr.rpm dotnet-runtime.rpm \
    && rm dotnet-host.rpm dotnet-hostfxr.rpm dotnet-runtime.rpm
