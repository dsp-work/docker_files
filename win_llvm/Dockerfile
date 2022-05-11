#escape=`
FROM mcr.microsoft.com/windows:20H2
USER ContainerAdministrator

WORKDIR C:/Downloads
ADD https://aka.ms/vs/16/release/vc_redist.x64.exe C:/Downloads/vcredist_x64.exe
RUN C:\Downloads\vcredist_x64.exe /install /passive /norestart /log out.txt


ADD [ "https://github.com/microsoft/winget-cli/releases/latest/download/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle", "C:\\TEMP\\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.zip" ]
RUN powershell -Command "Expand-Archive -LiteralPath C:\TEMP\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.zip -DestinationPath C:\TEMP\winget-cli -Force"
RUN ren C:\TEMP\winget-cli\AppInstaller_x64.msix AppInstaller_x64.zip
RUN powershell -Command "Expand-Archive -LiteralPath C:\TEMP\winget-cli\AppInstaller_x64.zip -DestinationPath C:\TEMP\winget-cli\ -Force"
RUN mkdir "C:\Program Files\winget-cli" 
#&& \
RUN copy "C:\TEMP\winget-cli\\winget.exe" "C:\Program Files\winget-cli\winget.exe" 
#&& `
RUN copy "C:\TEMP\winget-cli\\resources.pri" "C:\Program Files\winget-cli\"