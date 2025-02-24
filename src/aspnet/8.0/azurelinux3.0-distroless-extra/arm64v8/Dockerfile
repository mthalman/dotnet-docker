ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM mcr.microsoft.com/azurelinux/base/core:3.0 AS installer

RUN tdnf install -y \
        ca-certificates \
        gzip \
        tar \
    && tdnf clean all

# Retrieve ASP.NET Core
RUN aspnetcore_version=8.0.13 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-arm64.tar.gz \
    && aspnetcore_sha512='d67130310e81f727f1d4806463f49af18e012d6dc766c940838854922b3a3e7f7171c87d595c4dc09e1c63470fae3017b54d51be98618985129ff37d6a5ac0b3' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf aspnetcore.tar.gz -C /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:9.0.2-azurelinux3.0-distroless-extra-arm64v8@sha256:3c58ddbdd9c8f8c3aefd424a211bdc3d93024177d9964a99a91a5d5e549c5226

# ASP.NET Core version
ENV ASPNET_VERSION=8.0.13

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
