FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim-amd64 AS build
ARG TARGETARCH
RUN dpkg --add-architecture arm64 && \
    apt-get update && \
    apt-get install -y clang zlib1g-dev zlib1g-dev:arm64 gcc-aarch64-linux-gnu llvm
WORKDIR /source

COPY *.csproj .
RUN dotnet restore -r linux-$TARGETARCH

COPY . .
RUN dotnet publish -a $TARGETARCH --sc --no-restore -o /app
RUN rm /app/*.dbg


FROM mcr.microsoft.com/dotnet/runtime-deps:8.0
WORKDIR /app
COPY --from=build /app .
USER $APP_UID
ENTRYPOINT ["./releasesapi"]