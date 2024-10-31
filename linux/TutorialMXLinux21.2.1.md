# Tutorial MX Linux 21.2.1

## Personalizando

Para entrar sem senha, verifique se a linha "autologin-user=NOME_DO_USUÁRIO_AQUI" está descomentada no arquivo "/etc/lightdm/lightdm.conf". Em seguida, abra "Gerenciador de Usuários do MX" → Opções. Em "Mudar de usuário", escolha teu usuário, e marque a opção "Iniciar sessão automaticamente".

Você consegue alterar várias configurações pelo comando "dconf-editor". Por exemplo, para inverter a rolagem do touchpad, siga o caminho "org.gnome.desktop.peripherals.touchpad", que lista todas as configurações do touchpad, e então, habilite a opção "natural-scroll".

Para personalizar o explorador de arquivos, acesse seu menu "Editar → Preferências". Em "Comportamento", marque "Clique duplo" e "Abra novas instâncias do thunar como abas".

Para personalizar o menu iniciar (whisker), clique com botão direito na barra de tarefas → Painel → Preferências do painel. Em itens, clique em Menu Whisker → Editar. Em Aparência, gosto das opções "Mostrar como ícones em grade" e "Mostrar as dicas de contexto dos aplicativos", e deixar "Tamanho do ícone do aplicativo" como Normal. As demais, desmarco. Em Comportamento, desmarco a opção "Trocar categorias ao passar o mouse". Em Comandos, também marco as opções Reiniciar, Desligar e Suspender.

Abra o Teclado. Em Atalhos de aplicativos, adicione os seguintes atalhos (https://unix.stackexchange.com/a/543026/366802):
* xflock4 → super+l
* xfce4-session-logout --suspend → shift+super+l
* sh -c 'wmctrl -x -a Thunar.Thunar || thunar' → super+1
* sh -c 'wmctrl -x -a google-chrome.Google-chrome || google-chrome' → super+2
* sh -c 'wmctrl -x -a code.Code || code' → super+3
* sh -c 'wmctrl -x -a xfce4-terminal.Xfce4-terminal || xfce4-terminal' → super+4
* sh -c 'wmctrl -x -a libreoffice.libreoffice-startcenter || libreoffice' → super+5

Abra "Sessão e Inicialização" → "Início automático de aplicativos". Adicione o atalho `javaw -jar $HOME/Mega/NetBeansProjects/cpcb/target/cpcb-1.0-SNAPSHOT.jar # sem o w, caso esteja em unix`.