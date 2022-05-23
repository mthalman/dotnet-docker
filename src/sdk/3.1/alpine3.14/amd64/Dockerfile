ARG REPO=mcr.microsoft.com/dotnet/aspnet
# Installer image
FROM $REPO:3.1.26-alpine3.14 AS installer

ARG SAS_QUERY_STRING

# Install .NET Core SDK
RUN sdk_version=3.1.420-servicing-015994 \
    && wget -O dotnet.tar.gz https://dotnetclimsrc.blob.core.windows.net/dotnet/Sdk/$sdk_version/dotnet-sdk-3.1.420-linux-musl-x64.tar.gz$SAS_QUERY_STRING \
    && dotnet_sha512='5914afd5a91ebba30c33c390c8cd836fde9716b4a27b6fd7232d2c160a2401ede6dff92ae556eb1f0e1a2ed578c912d4dd65ff8afb063b83b31123bc4f871c21' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz


# .NET SDK image
FROM $REPO:3.1.26-alpine3.14

ENV \
    # Unset ASPNETCORE_URLS from aspnet base image
    ASPNETCORE_URLS= \
    # Do not generate certificate
    DOTNET_GENERATE_ASPNET_CERTIFICATE=false \
    # Disable the invariant mode (set in base image)
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip \
    # PowerShell telemetry for docker image usage
    POWERSHELL_DISTRIBUTION_CHANNEL=PSDocker-DotnetCoreSDK-Alpine-3.14

# Add dependencies for disabling invariant mode (set in base image)
RUN apk add --no-cache icu-libs

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]

# Trigger first run experience by running arbitrary cmd
RUN dotnet help

# Install PowerShell global tool
RUN powershell_version=7.0.11 \
    && wget -O PowerShell.Linux.Alpine.$powershell_version.nupkg https://pwshtool.blob.core.windows.net/tool/$powershell_version/PowerShell.Linux.Alpine.$powershell_version.nupkg \
    && powershell_sha512='dd8fc2981cbd7a2e7685c31c821d1d08a372d72dc4457428f78018cb63eb143b06f58afafd7d84717959a4fbe74eeba11fe93f2302ac6e4528caafa55932c8f2' \
    && echo "$powershell_sha512  PowerShell.Linux.Alpine.$powershell_version.nupkg" | sha512sum -c - \
    && mkdir -p /usr/share/powershell \
    && dotnet tool install --add-source / --tool-path /usr/share/powershell --version $powershell_version PowerShell.Linux.Alpine \
    && dotnet nuget locals all --clear \
    && rm PowerShell.Linux.Alpine.$powershell_version.nupkg \
    && ln -s /usr/share/powershell/pwsh /usr/bin/pwsh \
    && chmod 755 /usr/share/powershell/pwsh \
    # To reduce image size, remove the copy nupkg that nuget keeps.
    && find /usr/share/powershell -print | grep -i '.*[.]nupkg$' | xargs rm \
    # Add ncurses-terminfo-base to resolve psreadline dependency
    && apk add --no-cache ncurses-terminfo-base
