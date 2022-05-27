ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM amd64/buildpack-deps:bullseye-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.26-servicing.22274.3 \
    && curl -fSL --output aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-x64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='8bbf06012cdd2cff23c592e0d3c49d032d77add4dda8fba1d7ba73e6cc4ae97b1676908b14cdc7fc2fe723302e1efd27a44b48190a91d69c0e41bb5edb47501f' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-bullseye-slim

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.26-servicing.22274.3

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
