ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:3.1.26-cbl-mariner1.0 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22261.2 \
    && curl -fSL --output dotnet-host.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-host-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='3d4725774804ffb674981c8e6ac587971da8fb32703e9e2c3e6b671f7c3315ce2b1acec8103a08e30587fa2825483a2081d511b1bd9e6478f325f7e87940c3d0' \
    && echo "$dotnet_sha512  dotnet-host.rpm" | sha512sum -c - \
    \
    && curl -fSL --output dotnet-hostfxr.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-hostfxr-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='1a69b3f981dafb8096aabd8fe6208add5031c9e7b47b3594915f69591d9cba7d65b4c15c2f0fff3323139a917ed3ad5844a6f66f0db211803a8d4b0aca319aeb' \
    && echo "$dotnet_sha512  dotnet-hostfxr.rpm" | sha512sum -c - \
    \
    && curl -fSL --output dotnet-runtime.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && dotnet_sha512='fbc7570152853ee399d11f20d2034db07458ebcdeb598d800f17fd39b9f1c8ea5eaba5d5feb024cc0add7543ca650ed77268057b575074bab6357ed130884077' \
    && echo "$dotnet_sha512  dotnet-runtime.rpm" | sha512sum -c -


# .NET runtime image
FROM $REPO:3.1.26-cbl-mariner1.0

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22261.2

COPY --from=installer /dotnet-host.rpm /dotnet-host.rpm
COPY --from=installer /dotnet-hostfxr.rpm /dotnet-hostfxr.rpm
COPY --from=installer /dotnet-runtime.rpm /dotnet-runtime.rpm

RUN rpm --install dotnet-host.rpm dotnet-hostfxr.rpm dotnet-runtime.rpm \
    && rm dotnet-host.rpm dotnet-hostfxr.rpm dotnet-runtime.rpm
