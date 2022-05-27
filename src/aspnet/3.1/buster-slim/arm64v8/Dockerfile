ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM arm64v8/buildpack-deps:buster-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.26-servicing.22274.3 \
    && curl -fSL --output aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-arm64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='757ff6cbc31b1c8743077288d7fa621c73fa7f4d155d636ad100cda6e1f601e31d2f842d5cfef3dec5daa4c8c3efbcf76f02afd1c518cae7b67b2a46a9faab08' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-buster-slim-arm64v8

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.26-servicing.22274.3

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
