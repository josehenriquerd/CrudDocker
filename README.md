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



# Conectar-se à Máquina Virtual
Conecte-se à sua máquina virtual usando SSH. O comando pode ser semelhante ao seguinte, dependendo do seu sistema:

<code>ssh your_username@<public_ip_address></code>

#Instalar o Azure CLI na Máquina Virtual
<code>
sudo apt-get update
sudo apt-get install curl apt-transport-https lsb-release gnupg
curl -sL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
sudo apt-get update
sudo apt-get install azure-cli
sudo apt-get install -y apt-transport-https
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install -y dotnet-sdk-7.0
az --version
dotnet --version
</code>

#Instalar o Docker
<code>
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
sudo docker --version

</code>
