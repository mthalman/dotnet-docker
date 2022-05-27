ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm64v8/buildpack-deps:bionic-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22273.3 \
    && curl -fSL --output dotnet.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-linux-arm64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='574409616f5cbef35a2bd6fd1a2f0bcb3bdaa81457aea3af5e0e237ba768ced5214c51a3045697fe7478e8211e2045fc2072e382d6f456509a8f2923e9b1fc26' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:3.1.26-bionic-arm64v8

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22273.3

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
