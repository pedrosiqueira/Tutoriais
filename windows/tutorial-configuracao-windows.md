## Instalação de programas

Instale o gerenciador de pacotes chocolatey.org. Execute no powershell como admin:

`choco install 7zip audacity calibre clementine discord firefox gimp git googlechrome jdownloader libreoffice megasync microsoft-windows-terminal notepadplusplus obs-studio okular openjdk python qbittorrent smplayer steam virtualbox vlc vscode`

## comandos de terminal

    git config --global core.eol lf # use LF em vez de CRLF nos repositórios GIT https://stackoverflow.com/q/1889559/4072641
    git config --global core.autocrlf input # use LF em vez de CRLF nos repositórios GIT
    git config --global core.filemode false # ignora flags de permissões

## Desabilitar a busca na web no menu iniciar do Windows 10

[Execute no powershell](https://woshub.com/disable-web-search-windows-start-menu/):

```
if( -not (Test-Path -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\Explorer)){
New-Item HKLM:\SOFTWARE\Policies\Microsoft\Windows\Explorer
}
Set-ItemProperty -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\Explorer -Name "DisableSearchBoxSuggestions" -Value 1 -Type DWORD
```

## Outros

1. Desative a opção “snap” de Quando eu ajustar uma janela, mostrar o que posso ajustar ao lado dela”.
2. Altere o navegador padrão para o chrome.
3. Abra o `shell:startup` e adicione o script `abnt2-hotkeys.bat` com a linha `start "" D:\Mega\CodingProjects\PythonScripts\IndividualPythonScripts\abnt2-hotkeys.pyw`.
4. Abra o `shell:startup` e adicione o script `cpcb.bat` com a linha `start "" javaw -jar D:\Mega\CodingProjects\JavaProjects\cpcb\target\cpcb-1.0-SNAPSHOT.jar`.
5. Abra o `shell:startup` e adicione o script `concurso-prefeitura-tl.bat` com a linha `python D:\Mega\CodingProjects\PythonScripts\IndividualPythonScripts\concurso-prefeitura-tl.py`.

## Habilitar entrada sem senha

No Editor de Registro, vá em "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device", e altere o valor de "DevicePasswordLessBuildVersion" de 2 para 0. Reinicie o computador. No netplwiz, desabilite a opção de digitar senha ao entrar.