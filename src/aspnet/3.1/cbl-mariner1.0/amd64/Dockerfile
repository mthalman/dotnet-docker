ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM $REPO:3.1.26-cbl-mariner1.0 AS installer

ARG SAS_QUERY_STRING

# Retrieve ASP.NET Core
RUN aspnetcore_version=3.1.25-servicing.22219.10 \
    && curl -fSL --output aspnetcore.rpm https://dotnetclimsrc.blob.core.windows.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-3.1.26-x64.rpm$SAS_QUERY_STRING \
    && aspnetcore_sha512='df1a46b689b8764e1f8ec7d5db3a924491527db7f7a7dd6bcb0abb69d084e7db0ef379dc10620d3b02de1e6f3922efa47132053b0ec42f055e308fb40875410a' \
    && echo "$aspnetcore_sha512  aspnetcore.rpm" | sha512sum -c -


# ASP.NET Core image
FROM $REPO:3.1.26-cbl-mariner1.0

# ASP.NET Core version
ENV ASPNET_VERSION=3.1.25-servicing.22219.10

COPY --from=installer /aspnetcore.rpm /aspnetcore.rpm

RUN rpm --install aspnetcore.rpm \
    && rm aspnetcore.rpm
