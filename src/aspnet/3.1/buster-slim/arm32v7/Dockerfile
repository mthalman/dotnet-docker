ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM arm32v7/buildpack-deps:buster-curl AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.25-servicing.22219.10 \
    && curl -fSL --output aspnetcore.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-linux-arm.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='fc6ea45a5b89f7ff66a863e89b477d94b329a3f6e0b03b9dfacc31a85a0b915f4f68aade825bbef1e0bb0f4b6fe7c173be7cb24b5cef685217d6a3919598d465' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:3.1.26-buster-slim-arm32v7

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.25-servicing.22219.10

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
