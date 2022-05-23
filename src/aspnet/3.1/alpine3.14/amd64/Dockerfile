ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:3.1.26-alpine3.14 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.25-servicing.22219.10 \
    && wget -O aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-musl-x64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='1f8c12190a16fff5a8454262cd0e9bed7660ad63278c34d2935889ed043a56130a11dc2167c84277d4cdaee1a292a77b52114f411781a0bae74f72d663a9e05c' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-alpine3.14

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.25-servicing.22219.10

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
