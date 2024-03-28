Vagrant.configure("2") do |config|
  config.vm.box = "gusztavvargadr/windows-server-2022-standard"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "12288" # 12GB de RAM
    vb.cpus = 4 # 4 CPUs
  end

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--vram", "100"] # 100GB de espacio en disco
  end

  config.vm.synced_folder "C:/BI", "/BI"
  config.vm.boot_timeout = 3000
  # Download installers
  config.vm.provision "shell", inline: <<-SHELL
    # Install PowerShell if not already installed (for older Windows versions)
    if (!(Test-Path "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe")) {
      dism.exe /Online /Enable-Feature /FeatureName:MicrosoftWindowsPowerShell /All /NoRestart
    }
    # Create a directory to store the downloaded file if it doesn't exist
    mkdir -p C:/BI/installers
    # Download the file using curl
    curl -o C:/BI/installers/SQL2022-SSEI-Dev.exe "https://go.microsoft.com/fwlink/p/?linkid=2215158&clcid=0x409&culture=en-us&country=us"
    curl -o C:/BI/installers/SSMS-Setup-ENU.exe "https://aka.ms/ssmsfullsetup"
    curl -o C:/BI/installers/Git-2.44.0-64-bit.exe "https://github.com/git-for-windows/git/releases/download/v2.44.0.windows.1/Git-2.44.0-64-bit.exe"
    curl -o C:/BI/installers/VSCodeUserSetup-x64-1.87.2.exe "https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-user"
    curl -o C:/BI/installers/VisualStudioSetup.exe "https://c2rsetup.officeapps.live.com/c2r/downloadVS.aspx?sku=community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030:4eda9cef6e4d4a8d9ac47a811b7b2bec"
  SHELL
  # Installing programs
  config.vm.provision "shell", inline: "powershell -Command 'C:\\BI\\installers\\SQL2022-SSEI-Dev.exe /ENU /IAcceptSqlServerLicenseTerms /Quiet /Verbose /ConfigurationFile=C:\\BI\\installers\\config\\ConfigurationFile.ini'"
  config.vm.provision "shell", inline: "powershell -Command 'C:\\BI\\installers\\SSMS-Setup-ENU.exe /install /quiet /norestart'"
  config.vm.provision "shell", inline: "powershell -Command 'C:\\BI\\installers\\Git-2.44.0-64-bit.exe /SP /VERYSILENT /NOCANCEL /NORESTART'"
  config.vm.provision "shell", inline: "powershell -Command 'C:\\BI\\installers\\VSCodeUserSetup-x64-1.87.2.exe /SP /VERYSILENT /NOCANCEL /NORESTART'"
  config.vm.provision "shell", inline: "powershell -Command 'C:\\BI\\installers\\VisualStudioSetup.exe --quiet --norestart'"
end