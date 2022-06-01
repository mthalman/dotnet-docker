ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:6.0.6-alpine3.14-arm32v7 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=6.0.6-servicing.22268.5 \
    && wget -O aspnetcore.tar.gz https://dotnetbuilds.blob.core.windows.net/internal/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-6.0.6-linux-musl-arm.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='ae31eb4e06fc182c799ce41822c3bf03ddd4a948f96691b7559a5669dbc481d5177c6ed383c9d86baf3362ed297911f5f7cca22f14b945a7b20e90f6052621bc' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:6.0.6-alpine3.14-arm32v7

# ASP.NET Core version
ENV ASPNET_VERSION=6.0.6-servicing.22268.5

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
