ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:bullseye-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve .NET Runtime
RUN dotnet_version=3.1.26-servicing.22261.2 \
    && curl -fSL --output dotnet.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-3.1.26-linux-x64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='381797d1e70790131924e3ab25df3eeba7ade1c0113b854fd05f9eb13cfb6569c654f258fe162fb854fe21d6a9f22e3523a847bd48a57f1eefb71d28a8a949c5' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:3.1.26-bullseye-slim

# .NET Runtime version
ENV DOTNET_VERSION=3.1.26-servicing.22261.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
