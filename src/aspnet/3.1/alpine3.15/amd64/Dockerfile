ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:3.1.26-alpine3.15 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.26-servicing.22274.3 \
    && wget -O aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-musl-x64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='7c0aedbdcb400df6d33fff31ca63f468f6440cfe3630f7181a52bc8953d4c309eb75215a1050ca41a1fda8f0bb826d6ebfe3b9ce82f30d4b438030a6e28cd755' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-alpine3.15

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.26-servicing.22274.3

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
