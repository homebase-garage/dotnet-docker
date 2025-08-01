ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM arm64v8/buildpack-deps:bookworm-curl AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=9.0.7 \
    && curl --fail --show-error --location \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-arm64.tar.gz \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/checksums/$aspnetcore_version-sha.txt \
    && sed -i 's/\r$//' $aspnetcore_version-sha.txt \
    && sha512sum -c $aspnetcore_version-sha.txt --ignore-missing \
    && mkdir --parents /dotnet \
    && tar --gzip --extract --no-same-owner --file aspnetcore-runtime-$aspnetcore_version-linux-arm64.tar.gz --directory /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm \
        aspnetcore-runtime-$aspnetcore_version-linux-arm64.tar.gz \
        $aspnetcore_version-sha.txt


# ASP.NET Core image
FROM $REPO:9.0.7-bookworm-slim-arm64v8

# ASP.NET Core version
ENV ASPNET_VERSION=9.0.7

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
