# Tutorial de instalação do antiX23-core

## Introdução

O antiX é um sistema operacional baseado no Debian, isto é, você pode usar o repositório do Debian e instalar aplicativos com apt. Entretanto o antiX não vem com o systemd. Você pode baixar do site https://antixlinux.com/download/ quatro “sabores” diferentes: `full`, `base`, `core` e `net`. O antiX-net é o menor de todos, vem apenas com linha de comando e sem drivers de wifi. Se você quiser baixar algo por ele, precisa estar conectado à internet via cabo. O antiX-core vem apenas com linha de comando também, mas com uma variedade de drivers como de wifi. O antiX-base e o antiX-full vêm com interface gráfica e alguns aplicativos já instalados, sendo que o full vem com mais aplicativos que o base.

Nesse tutorial instalaremos o antiX-core 23 x64 (27/08/2023) ao lado do Windows. Para isso, anteriormente é recomendado que você já tenha disponibilizado pelo Windows um espaço no HD para a instalação do Linux. Você também precisa saber se teu HD está no modo MBR, GPT com ou sem UEFI.

## Começando

Ao iniciar o antiX pela mídia de instalação, aparecerá o menu de inicialização. Escolha as opções de idioma, teclado e localização (language – keyboard – timezone).

Depois é só escolher a opção “antiX-23 x64-core e teclar [Enter]. O sistema será inicializado com as opções escolhidas. Será solicitado um nome de usuário e senha, digite `root` para ambos.

## Acessando a internet via linha de comando

Você pode conectar seu computador à internet conectando um cabo de rede, ou então conectando a uma rede wifi. Para conectar a uma rede wifi, execute `ceni` → `wlan0` → `scan` (pode demorar um pouco)→ escolha tua rede e insira a senha → Em `Class`, escolha `auto`.

Para verificar se está conectado à internet, execute `ping 1.1.1.1` e veja se consegue receber dados do servidor Cloudfare. Para sair do programa `ping`, tecle [Ctrl]+[C].

## Instalação do antiX-core

Se teu HD estiver no formato GPT com UEFI, é necessário acessar a internet para instalar alguns pacotes. Siga os passos em [Acessando a internet](#acessando-a-internet-via-linha-de-comando). Execute `apt update && apt install grub-efi`. Pronto, agora temos o pacote para instalar o grub em UEFI.

Execute `cli-installer` para iniciar a instalação.

*Você quer particionar?* `y`. Vamos criar o swap, uma extensão da RAM, para deixar o sistema mais rápido. Selecione o espaço livre → `new`. `Partition size` geralmente é a metade da RAM. Selecione `primary`. Selecione a partição criada → `type`. Selecione `82 Linux swap`. Vamos criar o root, a partição onde ficará o Linux. Selecione o espaço livre → `new`. `Partition size` é tudo o que sobrou. Selecione `primary`. Agora anote a partição que é marcada como `Boot` ou como `EFI`, vamos usá-la depois. Salve as alterações em `Write`. Digite `yes` para confirmar. Selecione `quit` para prosseguir com a instalação.

Vamos instalar o linux na partição `root` recém-criada. Digite o nome da última partição criada. Escolha `ext4`. *Você quer usar uma partição ‘/home’ separada*? `n`. *Do you wish to preserve the data?* `n`. *The installar will now destroy…* `y`. *O sistema em execução é o antiX-net?* `n`.

*Você quer instalar alguns pacotes?* tecle `y` se tiver acesso à internet. Vamos aproveitar para atualizar o kernel. *Executar apt-get update agora?* `y` → *Ignore estas atualizações por enquanto* → *Procurar núcleos (kernels) do antiX* → *exibir XX resultados primeiro* → digite um número correspondente à versão mais recente disponível do kernel → instalar o pacote → *Você quer instalar este pacote agora?* `y` → *Pressione <Enter> para continuar* → Sair.

*Você quer continuar?* `y`. *Where should grub be installed?* Escolha `MBR` se teu HD estiver em tal modo; Escolha `root` se teu HD estiver no modo GPT sem UEFI; Escolha `ESP` se teu HD estiver no modo GPT com UEFI. Caso tenha escolhido `ESP`, aparecerão mais duas opções: *Format the ESP partition?* ATENÇÃO: ESCOLHA `N` CASO SEJA DUAL BOOT COM WINDOWS, SENÃO VOCÊ PERDE O BOOT DO WINDOWS! *Do you want the UEFI boot NVRAM updated?* `y`.

*Nome do computador?* O nome do computador é o que aparece quando ele é listado numa rede web. *Você quer configurar a localização do sistema?* `n`. *Você quer configurar o teclado?* `y`, configure seu teclado como ABNT, se for o caso. *Você quer configurar o esquema/leiaute do console?* `n`. *Você quer configurar o fuso horário do sistema?* `y`, escolha conforme o da tua cidade. *Esta é uma instalação remasterizada/imagem de disco (snapshot)?* `n`. Insira teu nome de usuário e senha. Informe o novo valor ou pressione [Enter] para aceitar o padrão. *A informação está correta?* `y`. Insira a senha para root (administrador). *Definir entrada automática?* `y`.

Pronto, Linux instalado com sucesso! Agora, reinicie executando `reboot`.

## O grub

Sempre que ligar/reiniciar o computador, aparecerá a tela do grub, onde você seleciona qual sistema operacional será iniciado. O grub é chamado de *bootloader*, um programa que reconhece os sistemas operacionais instalados no teu HD. Se você instalou o antiX ao lado do windows, mas o windows não estiver aparecendo no grub, vamos adicioná-lo depois. Selecione o antiX para iniciá-lo.

## Atualizando o kernel

Se você já atualizou o kernel no processo de instalação, não precisa fazer esses passos, prossiga para o próximo parágrafo, para desinstalar a versão antiga do kernel. Se você não atualizou o kernel e deseja fazer agora, siga esses passos. Verifique se está [conectado à internet](#acessando-a-internet-via-linha-de-comando). Execute `sudo cli-aptiX`. Siga os passos: Executar apt-get update agora? Não → Procurar núcleos (kernels) do antiX → exibir XX resultados primeiro → digite um número correspondente à versão mais recente disponível do kernel → instalar o pacote → você quer instalar este pacote agora? Sim → Pressione <Enter> para continuar → Sair. Execute `sudo reboot` para reiniciar o computador.

Na próxima vez que aparecer o GRUB, selecione a opção “advanced options…” e verifique se o kernel mais recente foi instalado, selecionando-o em seguida. Ou então, se você já entrou no sistema, execute `uname --r` para saber o kernel em execução no momento. Agora, vamos desinstalar o kernel mais antigo para não ficar ocupando espaço. Digite, sem executar ainda, o comando `sudo apt remove linux-image-`, seguido da tecla [Tab] duas vezes, para listar os kernels atualmente instalados. Complete o comando com o número da versão que você quer desinstalar. Também acrescente ao comando o pacote que começa com `linux-header`, teclando [Tab] duas vezes para listar os headers dos kernels instalados, e complete o comando com o número da versão que quer desinstalar. No meu caso, o comando completo ficou assim: `sudo apt remove linux-image-5.10.188-antix.1-amd64-smp linux-headers-5.10.188-antix.1-amd64-smp`. Agora, pode executar o comando. Para mais informações sobre como remover kernels do linux: https://www.cyberciti.biz/faq/debian-redhat-linux-delete-kernel-command/.

## Instalação de uma interface gráfica

Nesta seção, faremos algumas configurações no antiX-core, e também instalaremos a interface gráfica LXDE, por ser muito leve e versátil. Para isso, verifique se está [conectado à internet](#acessando-a-internet-via-linha-de-comando).

Basicamente, os comandos que executo nesta fase são os seguintes:

    sudo update-grub && echo 'set completion-ignore-case On' | sudo tee -a /etc/inputrc && sudo sed -i '/^[^#]/s/^/# /' /etc/locale.gen && sudo sed -i '/\(en_US\|pt_BR\).UTF-8/s/^# //' /etc/locale.gen && echo 'Informe a data e hora atual (aaaa-mm-dd hh:mm:ss):' && read currentdatetime && sudo hwclock --set --date="$currentdatetime" && sudo hwclock --hctosys && sudo hwclock --systohc --localtime && sudo apt -y update && sudo apt -y dist-upgrade && sudo apt -y install xorg lightdm lxde obconf xdg-utils connman-gtk gnome-disk-utility gnome-screenshot xscreensaver lxhotkey-plugin-openbox lxhotkey-data lxsession-default-apps openssh-client smbclient cifs-utils winbind sshfs fuseiso gvfs-backends gvfs-fuse jmtpfs gphotofs ifuse exfat-fuse cups gnome-keyring rofi xdotool aspell-en aspell-pt-br birdtray build-essential calibre clementine ddd ffmpeg firefox fonts-noto gdb gimp git hyphen-pt-br libglib2.0-bin libreoffice obs-studio okular openjdk-21-doc openjdk-21-source playonlinux python3-full python3-dev python3-pip python3-doc recoll smplayer ttf-ancient-fonts unrar vlc qbittorrent virtualbox xclip && curl https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb --output google-chrome.deb && sudo dpkg -i ./google-chrome.deb ; sudo apt -y install -f && rm google-chrome.deb && echo -e '#!/bin/bash\napt update\napt dist-upgrade -y\napt autoclean\napt autoremove' | sudo tee /etc/cron.weekly/autoupdupgr >/dev/null && sudo chmod +x /etc/cron.weekly/autoupdupgr && sudo halt

Sinta-se à vontade para executá-los, mas se você quiser saber mais a respeito do que esses comandos fazem, prossiga nesta seção.

Se você tem o windows e no grub não estiver aparecendo o windows, execute `sudo update-grub`.

Para o terminal [não diferenciar maiúsculas de minúsculas](https://askubuntu.com/a/87066/586845) (case insensitive) ao autocompletar com a tecla [Tab], execute `echo 'set completion-ignore-case On' | sudo tee -a /etc/inputrc`.

Por padrão, o antiX vem com várias configurações de localização que não utilizo. Eu prefiro deixar apenas português brasileiro e inglês americano. Para isso, execute `sudo sed -i '/^[^#]/s/^/# /' /etc/locale.gen` para desabilitar todos os idiomas, seguido de `sudo sed -i '/\(en_US\|pt_BR\).UTF-8/s/^# //' /etc/locale.gen` para habilitar apenas os idiomas pt_BR e en_US.

Sistemas Linux/Unix/Mac armazenam o horário do hardware (BIOS) como UTC por padrão, mas o Microsoft Windows armazenem o horário do hardware como a hora 'local'. Isso ocasiona sincronização errada do horário em dual boot. Você pode [alterar o Windows](https://askubuntu.com/a/169384/586845) para armazenar o horário da BIOS como UTC. Mas é mais fácil [alterar o Linux](https://askubuntu.com/a/720466/586845) para usar o horário da BIOS como local. Para isso, caso seja dualboot com windows, execute `echo 'Informe a data e hora atual (aaaa-mm-dd hh:mm:ss):' && read currentdatetime && sudo hwclock --set --date="$currentdatetime" && sudo hwclock --hctosys && sudo hwclock --systohc --localtime`. Essa sequência de comandos, respectivamente, solicita para você digitar a data e hora atual, grava a data e hora que você digitou, define o horário da BIOS com o que você digitou, sincroniza o horário do sistema com o da BIOS, e define o horário da BIOS como local. Manter o horário correto e sincronizado é essencial para conseguir atualizar os pacotes, que faremos a seguir.

Atualize os pacotes do sistema executando `sudo apt update && sudo apt dist-upgrade`.

Instale a interface gráfica juntamente com programas auxiliares (esse comando demora!) executando:

    sudo apt -y install xorg lightdm lxde obconf xdg-utils connman-gtk gnome-disk-utility gnome-screenshot xscreensaver lxhotkey-plugin-openbox lxhotkey-data lxsession-default-apps openssh-client smbclient cifs-utils winbind sshfs fuseiso gvfs-backends gvfs-fuse jmtpfs gphotofs ifuse exfat-fuse cups gnome-keyring rofi xdotool

O que é cada pacote? Xorg: servidor para Sistema de Janelas X. Lightdm: gerenciador de login gráfico. Lxde: ambiente de desktop leve. Obconf: Openbox Configuration Manager, para configurar o Openbox, gerenciador de janelas padrão do lxde. Xdg-utils: utilitários para integrar aplicativos com o ambiente de desktop, como o xdg-open. Connman-gtk: gerenciador de rede com suporte à bandeja do sistema. Gnome-disk-utility: gerenciador de discos. Gnome-screenshot: ferramenta de screenshot. Xscreensaver: Este pacote inclui o mínimo necessário para apagar e bloquear sua tela. Lxhotkey-plugin-openbox: gerenciador de atalhos do lxde. Lxhotkey-data: arquivos de internacionalização para o gerenciador de atalhos do lxde. Lxsession-default-apps: utilitário para configurar o lxsession e seus aplicativos padrões. Openssh-client: conecta a outros computadores via ssh. Smbclient: conecta a outros computadores via samba, especialmente útil para se conectar a windows. Cifs-utils: utilitário para o samba. Winbind: compartilha pastas com o windows. Sshfs: permite montar pastas remotas via ssh. Fuseiso: permite montar imagens ISO. Gvfs-backends: Usado pelo explorador de arquivos do lxde para montar automaticamente drives conectados ao computador. gvfs-fuse: auxiliar do gvfs para drives do tipo fuse. Jmtpfs: permite montar pastas usando MTP, útil para conectar Android ao computador. Gphotofs: permite montar pastas usando PTP, útil para conectar câmeras com armazenamento interno. Ifuse: permite montar pastas de iPhone. exfat-fuse: para montar drives exfat. Cups: pacote de sistema de impressão Unix. Gnome-keyring: gerenciador de senhas, usado por aplicativos como o Visual Studio Code. Rofi: um lançador de aplicativos e janelas, mais avançado que o dmenu. Xdotool: controlador de janelas, teclado e mouse.

Esses são pacotes que utilizo, opcionais. Sinta-se à vontade para escolher os que lhe convém:

    sudo apt -y install aspell-en aspell-pt-br build-essential calibre clementine ddd ffmpeg firefox fonts-noto gdb gimp git hyphen-pt-br libglib2.0-bin libreoffice obs-studio okular openjdk-21-doc openjdk-21-source playonlinux python3-full python3-dev python3-pip python3-doc recoll smplayer ttf-ancient-fonts unrar vlc qbittorrent virtualbox xclip

O que é cada pacote? aspell-en: corretor ortográfico para inglês usado em vários programas, principalmente de linhas de comando. aspell-pt-br: corretor ortográfico para português. build-essential: lista de pacotes para compilar programas na linguagem C. calibre: Gerenciador de e-books. clementine: player de música avançado. ddd: interface gráfica para o gdb. ffmpeg: editor de vídeo/áudio por linha de comando. firefox: navegador de internet. fonts-noto: collection of font families to show chars like ★. gdb: Debugger para linguagem C. gimp: editor de imagens. git: sistema de controle de versões. hyphen-pt-br: padrões de hifenização em português para o libreoffice. libglib2.0-bin: só assim para o vscode conseguir mover um arquivo para a lixeira. libreoffice: pacote de escritórios. obs-studio: gravador e streamer de vídeo. okular: leitor avançado de pdf. openjdk-21-doc: documentação da última versão LTS do java até o momento. openjdk-21-source: última versão LTS do JDK até o momento. playonlinux: interface gráfica para o wine, instalador de softwares do Windows. python3-full python3-dev python3-pip python3-doc: suporte completo ao python, como pip e venv. recoll: indexador e buscador de textos em arquivos, como o grep. smplayer: player de vídeo avançado. ttf-ancient-fonts: collection of font families to show chars like 😊. unrar: para extrair arquivos RAR. vlc: outro player de vídeo avançado. qbittorrent: baixador de torrents. virtualbox: gerenciador de máquinas virtuais. xclip: controla por linha de comando a área de transferência (clipboard).

O google-chrome não está disponível na loja apt. Se você usa o google-chrome, [pode instalá-lo](https://linuxize.com/post/how-to-install-google-chrome-web-browser-on-debian-10/) executando:

    curl https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb --output google-chrome.deb && sudo dpkg -i ./google-chrome.deb ; sudo apt install -f && rm google-chrome.deb

Essa sequência de comandos, respectivamente, baixa o instalador do chrome, executa o instalador, e exclui o instalador após o chrome ter sido instalado.

Para [automaticamente atualizar os pacotes do linux](https://help.ubuntu.com/community/AutoWeeklyUpdateHowTo#Advanced_Alternative:_Automatic_Weekly_Package_Updates_Using_Cron_And_Apt-Get) semanalmente, podemos salvar um script com os comandos de atualização na pasta `/etc/cron.weekly`. Essa pasta é gerenciada pelo programa `cron`, que executa semanalmente todos os scripts dentro dela.

    echo -e '#!/bin/bash\napt update\napt dist-upgrade -y\napt autoclean\napt autoremove' | sudo tee /etc/cron.weekly/autoupdupgr >/dev/null && sudo chmod +x /etc/cron.weekly/autoupdupgr

Por fim, desligue teu computador executando `sudo halt`!

## Configurando o antiX-core 23

Como instalamos o `connman-gtk` para gerenciar a rede de internet, se você usou o `ceni` para conetar a uma rede wifi, desabilite a rede wifi pelo ceni [para não conflitar com o connman](https://wiki.archlinux.org/title/ConnMan#Error_.2Fnet.2Fconnman.2Ftechnology.2Fwifi:_No_carrier), executando `sudo ceni` → `wlan0` → remover configuração.

Os principais comandos do gerenciador de rede de internet são:

- `sudo connmanctl technologies`: Verifica o status do wifi.
- `sudo connmanctl disable wifi`: Desabilita o wifi.
- `sudo connmanctl enable wifi`: Habilita o wifi. Depois que habilita o wifi, é bom reiniciar o connman com `sudo service connman restart`
- `sudo connmanctl scan wifi`: Escanea por redes wifi.
- `sudo connmanctl services`: Lista as redes disponíveis.
- `sudo connmanctl connect <service_name>`: Conecta à `<service_name>`, que é o nome da rede disponível listada no comando anterior.

Na primeira vez que se conecta a uma rede, que precisa de senha, execute `sudo connmanctl`. Dentro do connmanctl, execute `agent on`, para poder inserir senha. Depois, se conecte com `connect <service_name>`, como explicado anteriormente. A propósito, os demais comandos também funcionam aqui.

Você pode se conectar a uma rede wifi também através da interface gráfica com `connman-gtk`. Abra o menu *settings* → habilite *use status icon* e *launch to tray by defaults*. Habilite o wifi e entre na tua rede de internet.

Para conectar o connman a uma rede peap, é necessário [fazer manualmente](https://unix.stackexchange.com/a/509074/366802). Execute `sudo mousepad /var/lib/connman/eduroam.config` para criar um arquivo de configuração de rede peap, e coloque o seguinte conteúdo no arquivo, substituindo os campos apropriados:

    [service_eduroam]
    Type=wifi
    Name=<nome-da-rede>
    EAP=peap
    Phase2=MSCHAPV2
    Identity=<login>
    Passphrase=<password>

Preencha os campos sem colocar aspas. Depois, reinicie o serviço executando `sudo service connman restart`.

Para habilitar no touchpad, respectivamente, *tapping* e *natural scrolling*, execute `sudo mousepad /etc/X11/xorg.conf.d/synaptics.conf` e adicione/altere as seguintes linhas no arquivo:

    Option "Tapping" "on"
    Option "NaturalScrolling" "on"

Para habilitar login automático, execute `sudo mousepad /etc/lightdm/lightdm.conf` e adicione as seguintes linhas no final do arquivo, substituindo o campo apropriado pelo seu nome de usuário:

    [SeatDefaults]
    autologin-user=<YOUR USER>
    autologin-user-timeout=0

Para alterar as configurações do grub, execute `sudo mousepad /etc/default/grub`. O valor de `GRUB_TIMEOUT` é o tempo em que o grub esperará antes de iniciar o sistema padrão. O valor de `GRUB_DEFAULT` é o sistema padrão. Para colocar o windows como padrão, você deve verificar qual a posição dele na lista do grub, isto é, se ele está na primeira, segunda, ou outra posição. Supondo que ele esteja na quinta posição, você deve colocar o valor 4, sempre um a menos, pois, no grub, a primeira posição é o valor 0, e assim sucessivamente. Mais informações você pode [encontrar aqui](https://askubuntu.com/a/110738/586845). Depois execute `sudo update-grub` para aplicar as alterações.

Abra o menu iniciar → preferências → protetor de tela (xscreensaver). Na aba "Modos de exibição", gosto de deixar "ativar (blank) após 5 minutos", "circular (cycle) após 4 minutos", e "bloquear a tela após 1 minuto". Na aba "Avançado", gosto de ativar "Gerenciamento de energia ativado (power management enabled)", e deixar "standby after 10 minutes", "suspended after 240 minutes" e "off after 999 minutes".

## Personalizando o antiX-core 23 com LXDE

Nesta seção, personalizaremos o LXDE. Todas as personalizações ficam salvas praticamente dentro da pasta `~/.config`. Como eu já tenho as minhas personalizações, apenas extraio o conteúdo da pasta `lxdepersonalizado.tar.gz` na minha pasta pessoal e reinicio o computador. Sinta-se à vontade para seguir minha personalização, mas se você quiser fazer manualmente cada personalização, prossiga nesta seção.

Adicione as seguintes linhas em `~/.bashrc`:

    # minhas personalizacoes
    HISTSIZE=1000 # for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
    HISTFILESIZE=1000 # for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
    alias dif="diff --color -u" # atalho para o diff
    alias lll="ll -th | head -13" # exibe os 10 últimos arquivos modificados da pasta atual
    export t="$HOME/Downloads/t"
    export t1="$HOME/Downloads/t1"
    export t2="$HOME/Downloads/t2"
    export LESS="-I" # less agora ignora caixa por padrao
    # parece que o lxde+lightm não carrega o .profile, portanto, carrego a pasta .local/bin por aqui mesmo
    if [ -d "$HOME/.local/bin" ] && [[ ! "$PATH" == *"$HOME/.local/bin"* ]]; then
        PATH="$HOME/.local/bin:$PATH"
    fi

Copie os scripts de `lxdepersonalizado.tar.gz/.local/bin` para a pasta `~/.local/bin/`, garantindo que eles sejam executáveis, executando `chmod +x ~/.local/bin/*`.

Para personalizar a barra de tarefas, clique direito nela → Configurações do painel → miniaplicativos do painel. Eu gosto de deixar apenas:

* *menu*.
*  *barra de lançamento de aplicativos*. Clique duas vezes e adicione os apps desejados, como terminal.
*  *barra de tarefas*.
*  *monitor de recurso*. Clique duas vezes, e habilite os campos CPU e RAM.
*  *controle de volume*.
*  *área de notificação*.
*  *relógio digital*. Clique duas vezes, no campo “formato do relógio”, deixe “%d/%m %R” para exibir data e hora.
*  *minimizar todas as janelas*. Clique duas vezes, habilite “alternadamente iconificar/enrolar”.

Para alterar a aparência do terminal, clique no menu *Preferências* → *Exibir*. Gosto de deixar largura como 128, altura como 8 e *ocultar barra de menu*. Assim, o terminal ocupa pouco espaço. Caso necessário, para abrir a barra de menu, só abrir o menu de contexto (botão direito do mouse).

Para alterar as preferências do explorador de arquivos PCManFM, clique no menu *Editar* → *Preferências*. Na aba *Geral*, deixo *Modo de visão* como *lista detalhada*, e marco a opção "Don't ask options on launch executable file". Na aba *Layout*, habilito todas as opções de *mostrar nos locais*. Na aba *Advanced*, altero o *Terminal emulator* para `lxterminal %s`.

Para editar os atalhos, clique no menu *iniciar* → *preferências* → *configurar teclas de atalho*. Na aba *Ações*, crie os seguintes atalhos:

- alt+f7: mover.
- alf+f8: redimensionar.
- alt+f9: minimizar.
- alt+f10: alternar maximização.

Eu particularmente não trabalho com várias áreas de trabalho, logo, removo seus atalhos para não conflitar com outros que prefiro. Na aba *Programas*, eu particularmente removo o atalho ctrl+escape pois já tem um atalho para sua ação. Crie os seguintes atalhos:


| Atalho | Comando | Descrição |
| ------ | ------- | --------- |
| alt+ctrl+del | `lxde-logout` | Abre o menu de desligamento |
| super+l      | `sh -c 'xdg-screensaver lock & xset dpms force off'` | Bloqueia a tela com o `xscreensaver` e apaga a tela com o `xset` |
| super+f      | `sh -c 'xdg-open "$(locate -i $HOME/Mega \| rofi -threads 0 -dmenu -i -p "locate" -keep-right)"'` | Busca por um arquivo ou pasta |
| super+r      | `rofi -show drun` | Busca por um aplicativo |

Eu defini os atalhos [Super]+[um número] para abrir ou selecionar uma janela dos aplicativos que mais utilizo, que são, do 1 ao 5, respectivamente, explorador de arquivos, chrome, code, terminal e libreoffice. Caso eu já tenha alguma janela de um aplicativo aberta, posso abrir outra janela teclando [Shift]+[Super]+[número do aplicativo]. 

| Atalho | Comando |
| ------ | ------- |
shift+super+1 | pcmanfm |
shift+super+2 | google-chrome |
shift+super+3 | code |
shift+super+4 | lxterminal |
shift+super+5 | libreoffice |
super+1 | ~/.local/bin/windowselect.sh pcmanfm |
super+2 | ~/.local/bin/windowselect.sh google-chrome |
super+3 | ~/.local/bin/windowselect.sh code |
super+4 | ~/.local/bin/windowselect.sh lxterminal |
super+5 | ~/.local/bin/windowselect.sh libreoffice |

Sinta-se a vontade para mudar o atalho para seus aplicativos favoritos, ou até acrescentar mais na lista, você só precisa saber o nome de classe do aplicativo. Para isso, execute `xprop | fgrep CLASS` e clique na janela do aplicativo desejado.

Eu também gosto dos atalhos [Super]+[Seta] para mudar a janela atualmente focada de posição, recurso chamado de *snap window*. Para isso, crie os seguintes atalhos:

| Atalho | Comando |
| ------ | ------- |
| super+Left | ~/.local/bin/windowsnap.sh left |
| super+Right | ~/.local/bin/windowsnap.sh right |
| super+Up | ~/.local/bin/windowsnap.sh up |
| super+Down | ~/.local/bin/windowsnap.sh bottom |
| super+enter | ~/.local/bin/windowsnap.sh center |

No momento, esses atalhos de *snap window* estão com um bug em que não mudam de posição enquanto a janela está maximizada.

Também deixo na área de trabalho os atalhos dos programas que mais uso. Copie os atalhos de `lxdepersonalizado.tar.gz/Desktop/` para `~/Desktop`. Dentre os atalhos há dois que usam ícones localizados em `lxdepersonalizado.tar.gz/.local/share/icons/hicolor/`, que você também deve copiar, criando a pasta, se não houver. Caso os ícones não estejam renderizando, execute `gtk-update-icon-cache` e reinicie o computador.

## Extras

Instalações que não encontrei no apt (procure antes na loja de apps para manter atualizado automaticamente): vscode, megasync, jdownloader2, zotero (plugin do libreoffice para citações referências). Instalações pelo playonlinux: office 2010, notepad++.

Configurações extras:

- Adicionar configurações de impressora wireless. Se solicitar senha, informe teu nome e senha de usuário. Se não tiver permissão, execute `sudo usermod -a -G lpadmin your-username` conforme [esse link](https://unix.stackexchange.com/a/513983/366802).
- Instale o epson iProjection
- No git, adicionar as seguintes configurações:
    ```bash
    git config --global core.pager "less -R -F -X" # qual programa o git utiliza para exibir o diff https://stackoverflow.com/a/73417842/4072641
    git config --global pull.rebase true # como lidar com merges https://stackoverflow.com/a/16666418/4072641
    git config --global user.name "$(read -p 'Enter Git user name: ' username; echo "$username")" # define o usuário
    git config --global user.email "$(read -p 'Enter Git user email: ' useremail; echo "$useremail")" # define o e-mail do usuário
    ```
- No Mega, excluir as seguintes pastas da sincronização: `node_modules`, `__pycache__`.
- No Recoll, configurações de índice:
  - Global parameters:
    - Top directories: ~/Mega
    - Stemming languages: english, portuguese
    - Aspell language: pt_BR
  - Local parameters:
    - Skipped names: `.*`, `node_modules`, `__pycache__`
- No arquivo `/etc/updatedb.conf`, atualize a propriedade `PRUNEPATHS` para "/bin /boot /dev /etc /home/. /home/siqueira/. /home/siqueira/Mega/.debris /lib /lib32 /lib64 /libx32 /media /mnt /opt /proc /root /run /sbin /srv /sys /tmp /usr /var", e a propriedade `PRUNENAMES` para ".git .bzr .hg .svn node_modules \_\_pycache\_\_".

Instalações que não encontrei no apt que eu possa utilizar: [Udeler](https://github.com/FaisalUmair/udemy-downloader-gui), baixa vídeos do udemy. Dupeguru, encontra arquivos duplicados, similares, repetidos, diff.

Pacotes interessantes que já utilizei e posso voltar a utilizar:

    sudo apt install audacity gnome-tweaks handbrake handbrake-cli kazam mp3gain openssh-server picard postman python-is-python3 sox sqlitebrowser squirrelsql synaptic

## FAQ

Para fazer backup das personalizações de seu usuário, execute:

    mkdir -p lxdepersonalizado && rsync -a --exclude='.config/autostart' --exclude='.config/calibre' --exclude='.config/Code' --exclude='.config/dconf' --exclude='.config/GIMP' --exclude='.config/google-chrome' --exclude='.config/htop' --exclude='.config/libreoffice' --exclude='.config/Mousepad' --exclude='.config/menus/applications-merged' --exclude='.config/microsoft-edge' --exclude='.config/nvm' --exclude='.config/pulse' --exclude='.config/Recoll.org' --exclude='.config/VirtualBox' --exclude='.config/xarchiver' ~/.config ~/.bashrc ~/.profile lxdepersonalizado && rsync -av --include='*/' --include='.config/libreoffice/4/user/basic/Standard/*.xba' --include='.config/libreoffice/4/user/config/soffice.cfg/modules/*/toolbar/*.xml' --exclude='*' ~/.config lxdepersonalizado && find lxdepersonalizado -type d -empty -delete && tar -czf lxdepersonalizado.tar.gz lxdepersonalizado && rm -rf lxdepersonalizado

Não consigo usar atalhos que têm alt+shift. Geralmente o atalho alt+shift é mapeado para trocar o layout do teclado, o que impede de usar atalhos com alt+shift. Para [alterar ou desabilitar esse atalho](https://unix.stackexchange.com/a/333392/366802), abra `/etc/default/keyboard` e na linha `XKBOPTIONS="grp:alt_shift_toggle,grp_led:scroll"` retire a parte `grp:alt_shift_toggle`.

Meu drive não abre automaticamente (pendrive, CD, HD externo, …)… Acesse o gnome-disks através do menu *Acessórios* → *Discos*.

Iniciar programa junto com o sistema → Você pode criar um atalho na pasta `~/.config/autostart`.

Caso o windows esteja iniciando antes do grub https://askubuntu.com/questions/923136/how-to-set-grub-as-the-boot-manager.

Caso você acidentalmente formate a partição efi: https://woshub.com/how-to-repair-uefi-bootloader-in-windows-8/.

Caso você acidentalmente exclua a partição efi: https://woshub.com/how-to-repair-deleted-efi-partition-in-windows-7/.

O cabo Ethernet está desconectado e o boot demora até o timeout de "DHCP discover on eth0...": Comente as seguintes linhas do arquivo /etc/network/interfaces:

    #auto eth0
    #iface eth0 inet dhcp

## Por fazer

Alterar o menu iniciar, reorganizando os ícones para os que utilizo. Alternativa, colocá-los na área de trabalho. Apps no menu iniciar: word powerpoint ferramenta-captura resolve.

Relembrar posição das janelas quando fechá-las, para quando abri-las, aparecer na mesma posição.

atalho super+shift+seta para jogar uma janela para outra tela

atalho super+p para abrir configurações do monitor
