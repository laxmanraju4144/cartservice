
# FROM mcr.microsoft.com/dotnet/sdk:10.0 AS builder

# WORKDIR /app
# COPY cartservice.csproj .
# RUN dotnet restore cartservice.csproj \
#     -a x64
# COPY . .
# RUN dotnet publish cartservice.csproj \
#     -p:PublishSingleFile=true \
#     -a x64 \
#     --self-contained true \
#     -p:PublishTrimmed=true \
#     -p:TrimMode=full \
#     -c release \
#     -o /cartservice

# # https://mcr.microsoft.com/product/dotnet/runtime-deps
# FROM mcr.microsoft.com/dotnet/runtime-deps:10.0

# WORKDIR /app
# COPY --from=builder /cartservice .
# EXPOSE 7070
# ENV DOTNET_EnableDiagnostics=0 \
#     ASPNETCORE_HTTP_PORTS=7070
# USER 1000
# ENTRYPOINT ["/app/cartservice"]
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS builder

WORKDIR /app
COPY cartservice.csproj .
RUN dotnet restore cartservice.csproj
COPY . .
RUN dotnet publish cartservice.csproj \
    -p:PublishSingleFile=true \
    --self-contained true \
    -p:PublishTrimmed=true \
    -p:TrimMode=full \
    -c Release \
    -r linux-x64 \
    -o /cartservice

FROM mcr.microsoft.com/dotnet/runtime-deps:8.0

WORKDIR /app
COPY --from=builder /cartservice .
EXPOSE 7070
ENV DOTNET_EnableDiagnostics=0 \
    ASPNETCORE_HTTP_PORTS=7070

RUN adduser --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

ENTRYPOINT ["./cartservice"]
