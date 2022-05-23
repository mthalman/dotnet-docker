ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM $REPO:3.1.26-alpine3.14 AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22261.2 \
    && wget -O dotnet.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-linux-musl-x64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='1d29f533b42b7e8a4327077c510952af0f8064b866a1238993029b146d0e55512ca581468fe62fe3fad02e3cbbe9189b28a1b84d8638b830d957b66d22269ec0' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:3.1.26-alpine3.14

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22261.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
