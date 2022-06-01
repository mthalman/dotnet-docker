ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:6.0.6-cbl-mariner2.0-arm64v8 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=6.0.6-servicing.22268.5 \
    && curl -fSL --output aspnetcore.tar.gz https://dotnetbuilds.blob.core.windows.net/internal/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-6.0.6-linux-arm64.tar.gz$SAS_QUERY_STRING \
    && aspnetcore_sha512='a3bd7ce99ffb9b87766c49fcf28d802f4072af1d55f1d53ef4043a1a0b038a0fc8046669bbd82f64fb37e4c73703fa8f54a460caaa473d952baf941d23341c90' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c -


# ASP.NET Core image
FROM $REPO:6.0.6-cbl-mariner2.0-arm64v8

# ASP.NET Core version
ENV ASPNET_VERSION=6.0.6-servicing.22268.5

COPY --from=installer /aspnetcore.tar.gz /aspnetcore.tar.gz

RUN tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz
