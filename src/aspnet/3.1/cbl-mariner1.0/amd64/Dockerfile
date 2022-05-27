ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:3.1.26-cbl-mariner1.0 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.26-servicing.22274.3 \
    && curl -fSL --output aspnetcore.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && aspnetcore_sha512='1b7cc778ab15cf4fa221b42b2b3b764176ff1b77605b37f60bef07781cbca56f5ff288e066c1e34f814311a79ebf39a36124a4c73b77d05ce8e49c49e9b39126' \
    && echo "$aspnetcore_sha512  aspnetcore.rpm" | sha512sum -c -


# ASP.NET Core image
FROM $REPO:3.1.26-cbl-mariner1.0

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.26-servicing.22274.3

COPY --from=installer /aspnetcore.rpm /aspnetcore.rpm

RUN rpm --install aspnetcore.rpm \
    && rm aspnetcore.rpm
