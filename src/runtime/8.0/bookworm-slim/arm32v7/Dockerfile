ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm32v7/buildpack-deps:bookworm-curl AS installer

# Retrieve .NET Runtime
RUN dotnet_version=8.0.13 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-arm.tar.gz \
    && dotnet_sha512='5d018fb50eea96f3e9a56142202ccb1c73a725219d7257b0debbeef3ef196c69b076b98dfb6f098818c0514084df33f6704bc13a478d1727b012d6ba9eea0492' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:9.0.2-bookworm-slim-arm32v7@sha256:6b6abddacb54f6abeee7dcf488dd6bf83f4cbe1d32580f8a264626e2131ad844

# .NET Runtime version
ENV DOTNET_VERSION=8.0.13

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
