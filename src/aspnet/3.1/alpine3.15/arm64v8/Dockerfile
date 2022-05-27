ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:3.1.26-alpine3.15-arm64v8 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.26-servicing.22274.3 \
    && wget -O aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-musl-arm64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='655c7a0fa346a96b5ab5e64baec089ccf6333424f20dea043dbc195558278ecfe32129e5b33cf316d8cb4e4caa45c5310265368ac3cfbd97dcf9679709852fd9' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-alpine3.15-arm64v8

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.26-servicing.22274.3

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
