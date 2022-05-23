ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM arm64v8/buildpack-deps:bullseye-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.25-servicing.22219.10 \
    && curl -fSL --output aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-arm64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='003d0294aceaaa51adc2dd0d22fa645eba612e33c70504979b152f16f8fde8d0d07cee9056c5871a5773269f9724840f1618828620e1a31ae9b7ff6117c0336d' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-bullseye-slim-arm64v8

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.25-servicing.22219.10

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
