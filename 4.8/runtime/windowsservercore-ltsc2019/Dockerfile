# escape=`

FROM mcr.microsoft.com/windows/servercore:ltsc2019

# Install .NET 4.8
RUN powershell -Command `
        $ProgressPreference = 'SilentlyContinue'; `
        Invoke-WebRequest -Uri "https://download.visualstudio.microsoft.com/download/pr/7afca223-55d2-470a-8edc-6a1739ae3252/abd170b4b0ec15ad0222a809b761a036/ndp48-x86-x64-allos-enu.exe" -OutFile dotnet-framework-installer.exe & `
    .\dotnet-framework-installer.exe /q & `
    del .\dotnet-framework-installer.exe & `
    powershell Remove-Item -Force -Recurse ${Env:TEMP}\*

# Apply latest 
RUN curl -SL --output patch.msu http://download.windowsupdate.com/d/msdownload/update/software/secu/2019/05/windows10.0-kb4495618-x64_2d3245836cfd2467024a907947837e7879463a3b.msu `
    && mkdir patch `
    && expand patch.msu patch -F:* `
    && del /F /Q patch.msu `
    && DISM /Online /Quiet /Add-Package /PackagePath:C:\patch\Windows10.0-kb4495618-x64.cab `
    && rmdir /S /Q patch

# ngen .NET Fx
ENV COMPLUS_NGenProtectedProcess_FeatureEnabled 0
RUN \Windows\Microsoft.NET\Framework64\v4.0.30319\ngen update & `
    \Windows\Microsoft.NET\Framework\v4.0.30319\ngen update
