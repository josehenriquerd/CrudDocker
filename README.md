# CrudDocker

Api final, explicação e códigos:

Fizemos um CRUD completo, de duas entidades diferentes, utilizamos C# .NET, nossa solução utilizou banco MYSQL, incluso dentro da propria aplicação, Front-end, com Swagger para estarmos consumindo nossa API:

![image](https://github.com/user-attachments/assets/56bb4200-cd67-4f57-a8c5-455c0b8a2721)

Exemplos de Recuperação:

![image](https://github.com/user-attachments/assets/33afeacc-4755-437c-a730-d5a6a05e8efe)

Exemplo de Cadastro:

![image](https://github.com/user-attachments/assets/94266ea6-dc40-47bb-8165-c1753de9d553)

![image](https://github.com/user-attachments/assets/c8acafe1-1757-4b72-b675-79cbcc87b094)

Foi utilizado o .SLN pois se trata de uma solução .net

A construção do Docker File, foi feita a partir da IDE do Visual Studio:

![image](https://github.com/user-attachments/assets/9f2935d0-2e95-4c54-8fec-bffa4c1d3622)

API RODANDO NO DOCKER HUB:

![image](https://github.com/user-attachments/assets/601fb2dc-40d8-4055-8042-3a65591dd7cc)

![image](https://github.com/user-attachments/assets/3e7acbcf-a744-4c87-9938-6ab58175f1d2)

<h6>Configuração da aplicação na Azure:<h6>

Docker File:

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ScreenSound.API/ScreenSound.API.csproj", "ScreenSound.API/"]
COPY ["ScreenSound.Shared.Dados/ScreenSound.Shared.Dados.csproj", "ScreenSound.Shared.Dados/"]
COPY ["ScreenSound.Shared.Modelos/ScreenSound.Shared.Modelos.csproj", "ScreenSound.Shared.Modelos/"]
RUN dotnet restore "./ScreenSound.API/ScreenSound.API.csproj"
COPY . .
WORKDIR "/src/ScreenSound.API"
RUN dotnet build "./ScreenSound.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Esta fase é usada para publicar o projeto de serviço a ser copiado para a fase final
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ScreenSound.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Esta fase é usada na produção ou quando executada no VS no modo normal (padrão quando não está usando a configuração de Depuração)
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ScreenSound.API.dll"]




