# Create a container for running the OpenTelemetry ConfluentKafka integration tests.
# This should be run from the root of the repo:
# docker build --file test/OpenTelemetry.Instrumentation.ConfluentKafka.Tests/Dockerfile .

ARG BUILD_SDK_VERSION=9.0
ARG TEST_SDK_VERSION=9.0

FROM mcr.microsoft.com/dotnet/sdk:8.0.428@sha256:1b955f179b02df4f0eb5fadedaa898cbe44d620a64aab52eec72c8e91d1355b5 AS dotnet-sdk-8.0
FROM mcr.microsoft.com/dotnet/sdk:9.0.303@sha256:86fe223b90220ec8607652914b1d7dc56fc8ff422ca1240bb81e54c4b06509e6 AS dotnet-sdk-9.0

FROM dotnet-sdk-${BUILD_SDK_VERSION} AS build
ARG PUBLISH_CONFIGURATION=Release
ARG PUBLISH_FRAMEWORK=net9.0
WORKDIR /repo
COPY . ./
WORKDIR "/repo/test/OpenTelemetry.Instrumentation.ConfluentKafka.Tests"
RUN dotnet publish "OpenTelemetry.Instrumentation.ConfluentKafka.Tests.csproj" -c "${PUBLISH_CONFIGURATION}" -f "${PUBLISH_FRAMEWORK}" -o /drop -p:IntegrationBuild=true

FROM dotnet-sdk-${TEST_SDK_VERSION} AS final
WORKDIR /test
COPY --from=build /drop .
ENTRYPOINT ["dotnet", "vstest", "OpenTelemetry.Instrumentation.ConfluentKafka.Tests.dll"]
