ARG REPO=mcr.microsoft.com/dotnet/runtime
FROM $REPO:9.0.2-alpine3.21-amd64@sha256:6bc8d341b6c6b2521d2056793b0aef1e846a8f5927d74d4b2da48c4fb611f623

# .NET globalization APIs will use invariant mode by default because DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true is set
# by the base runtime-deps image. See https://aka.ms/dotnet-globalization-alpine-containers for more information.

# ASP.NET Core version
ENV ASPNET_VERSION=8.0.13

# Install ASP.NET Core
RUN wget -O aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$ASPNET_VERSION/aspnetcore-runtime-$ASPNET_VERSION-linux-musl-x64.tar.gz \
    && aspnetcore_sha512='f4992c2cbc8019378af0c8463d36c978caa943e53d49522037b802f7476ec9d82a6f69dfb2178f82f619bcedd6620ff0c1aba024760864eb0f73ce70c04fd8f9' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz -C /usr/share/dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz
