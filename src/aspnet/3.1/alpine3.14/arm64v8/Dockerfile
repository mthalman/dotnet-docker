ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:3.1.26-alpine3.14-arm64v8 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.25-servicing.22219.10 \
    && wget -O aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-musl-arm64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='aab5fbb9578354519737f495adbbf9ded777c4f20c21872912e60475515b30f62e130acb91d7c5dfa4575b37b393fc0f05cf8513844e4922ce7ff9acfd31ffe5' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-alpine3.14-arm64v8

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.25-servicing.22219.10

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
