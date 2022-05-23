ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM amd64/buildpack-deps:buster-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.25-servicing.22219.10 \
    && curl -fSL --output aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-x64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='6b0d39062374d5574cd5b52bbce033f7769f231494f9ff93ba6f1368195742eb1796f25268fee8a2f44ed34a44d822d11b8ff8de0ba06515e75b88c3cfbd0244' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-buster-slim

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.25-servicing.22219.10

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
