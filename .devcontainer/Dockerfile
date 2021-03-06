FROM mcr.microsoft.com/vscode/devcontainers/universal:linux

# Install .NET Core SDK
ENV DOTNET_SDK_DOWNLOAD_URL https://dotnetcli.blob.core.windows.net/dotnet/Sdk/3.1.402/dotnet-sdk-3.1.402-linux-x64.tar.gz

RUN sudo curl -SL $DOTNET_SDK_DOWNLOAD_URL --output dotnet.tar.gz \
    && sudo tar -zxf dotnet.tar.gz -C /home/codespace/.dotnet \
    && sudo rm dotnet.tar.gz
    
# Enable detection of running in a container
ENV DOTNET_RUNNING_IN_CONTAINER=true \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    # Skip extraction of XML docs - generally not useful within an image/container - helps perfomance
    NUGET_XMLDOC_MODE=skip

# Trigger the population of the local package cache
RUN sudo mkdir warmup \
    && cd warmup \
    && sudo dotnet new \
    && cd .. \
    && sudo rm -rf warmup \
    && sudo rm -rf /tmp/NuGetScratch

# Workaround for https://github.com/Microsoft/DockerTools/issues/87. This instructs NuGet to use 4.5 behavior in which
# all errors when attempting to restore a project are ignored and treated as warnings instead. This allows the VS
# tooling to use -nowarn:MSB3202 to ignore issues with the .dcproj project
ENV RestoreUseSkipNonexistentTargets false
