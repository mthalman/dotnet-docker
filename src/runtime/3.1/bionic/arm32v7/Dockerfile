ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm32v7/buildpack-deps:bionic-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22261.2 \
    && curl -fSL --output dotnet.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-linux-arm.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='c4894d02174fbea217a20e1dfb75f21a09e528eae4c7de27f41706f044bf11b0801553bb4d72bfbbc2a18d625ac6de954b87910b7e972b32335a0ec1f5b53d41' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:3.1.26-bionic-arm32v7

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22261.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
