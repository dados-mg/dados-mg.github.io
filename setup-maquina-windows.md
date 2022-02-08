Etapas para configurações máquinas Windows para publicação de dados abertos
===

- [ ] Autorizar usuário como administrador da máquina pois algumas instalações necessitam deste privilégio.
- [ ] Configurar proxy nas variáveis de ambiente.
  - [Link de referência](https://docs.cloudfoundry.org/cf-cli/http-proxy.html) 
  - Abra o prompt de comando e digite "sysdm.cpl".
  - Na aba "Advanced" selecione "Environment variables".
  - Na "system variables" clique em "new".
  - O padrão da variável a ser criada é `http://USER:PASSWORD@PROXYADDRESS:PORT`
  - Crie variável de nome http_proxy e valor http://m123456:123456@proxycamg.prodemge.gov.br:8080 (substitua m123456 por seu USER de rede e 123456 por sua PASSWORD)
  - Crie variável de nome https_proxy e valor http://m123456:123456@proxycamg.prodemge.gov.br:8080 (substitua m123456 por seu USER de rede e 123456 por sua PASSWORD). **CUIDADO**: O link da variável de ambiente "http**s**\_proxy" começa realmente com "**http**"
- [ ] Instalar editor de Texto de sua preferência.
  * Indicação: Sublime.
  * Pesquisar no google "sublime download for windows".
  * Realizar o download e instalação da versão mais recente disponível.
  * Importante instalar editor primeiro pois durante instalação git (próxima etapa) deverá ser selecionado o editor padrão.
- [ ] Configurar abertura Sublime via terminal.
 - [Vídeo de Referência](https://www.youtube.com/watch?v=MlnH8t4S4Qw).
 - [Link de Referência](https://stackoverflow.com/questions/9440639/sublime-text-from-command-line#:~:text=Add%20the%20installation%20folder%20to,cpl).
 - Incluir o caminho de instalação do Sublime no path do window:
   - Abra o prompt de comando e digite "sysdm.cpl".
   - Na aba "Advanced" selecione "Environment variables".
   - Na "system variables" chamada "Path" clique em "edit".
   - Adicione "C:\Program Files\Sublime Text3".
   - Salve e reinicie o prompt de comando.
 - Obs.: Selecione o caminho de instalação da sua máquina e não copie e cole o que os tutorais sugerem, pois poderá haver divergência.
 - Utilização: Navegando até a pasta desejada digite "subl ."
- [ ] Configurar atalho [Sublime auto-save](https://lucybain.com/resources/setting-up-sublime-autosave/).
- [ ] Configurar atalho para [comentar linhas](https://newbedev.com/shortcut-to-comment-out-a-block-of-code-with-sublime-text) no Sublime
- [ ] Instalar [git](https://git-scm.com/).
  -  Apenas a título de sugestão, visto que isso não atrapalhará em nada a instalação, como utilizaremos muito github, seguir com as opções padrão até "Adjusting the name of the initial branch in new repositories" e selecionar a opção "Override the default branch name for new repositories" com "main" na caixa de diálogo aberta para inserção da sua branch padrão. 
  -  Em "Configuring the terminal emulator to use with Git Bash" selecione "Use Windows' default console window"
  - Conferência de instalação realizada com sucesso:

```Git Bash Terminal
$ git --version

# Tipo de resposta esperada, podendo a versão ser diferente devido à data de instalação
git version 2.33.0.windows.2
```

- [ ] Configurar git para clone, pull, push via Proxy.
  - Clonar repositórios sempre visa https. 
  - [Link de Referência](https://stackoverflow.com/questions/18356502/github-failed-to-connect-to-github-443-windows-failed-to-connect-to-github). 

```Git Bash Terminal
# Modelo
# Não copie e cole cegamente, troque USER, PASSOWORD, PROXYADDRESS e PORT
$ git config --global http.proxy http://USER:PASSWORD@PROXYADDRESS:PORT

# Exemplo
$ git config --global http.proxy http://m123456:123456@proxycamg.prodemge.gov.br:8080
```

- [ ] Instalação [git large files - glf](https://git-lfs.github.com/)
  - [ ] Download e instalação de executável para Windows.
  - [ ] Finalização da instalação via terminal e conferência 

```Git Bash Terminal
# Instalação
git lfs install

# Tipo de resposta esperada, podendo a versão ser diferente devido à data de instalação
$ git lfs --version
git-lfs/2.13.3 (GitHub; windows amd64; go 1.16.2; git a5e65851)
```
- [ ] Fixar a senha no git para possibilitar todas as operações de clone, pull e push sem precisar digitar as credenciais em cada operação - digitar na bash:
````
$ git config --global credential.helper wincred
````

- [ ] Instalar Python 3.9
 - [Baixar e instalar Python 3.9](https://www.python.org/downloads/).
  - [Vídeo de Referência](https://www.youtube.com/watch?v=8aZkrsIQT3Y)
  - Muito importante na primeira tela da importação marcar a opção "Add Python 3.9 to PATH".
  - Selecionar a opção "Customize installation".
  - Em "Advanced Options" marque "Install for all users".
  - Conferência de instalação realizada com sucesso:

```Git Bash Terminal
$ python --version

# Tipo de resposta esperada, podendo a versão ser diferente devido à data de instalação
Python 3.9.6

# Criação e Ativação ambiente python Windows
# Criação
$ python -m venv venv

# Ativação
$ source venv/Scripts/activate
```

- Realizar login no [Portal de Dados Abertos - Produção](https://dados.mg.gov.br)
- Realizar login no [Portal de Dados Abertos - Homologação](https://homologa.cge.mg.gov.br)
- Realizar cadastro de [variáveis de ambiente Windows](https://www.architectryan.com/2018/08/31/how-to-change-environment-variables-on-windows-10/) para publicação de recursos nos ambientes de produção e homologação:
 - Produção:
  - CKAN_HOST_PRODUCAO e CKAN_KEY_PRODUCAO
 - Homologação:
  - CKAN_HOST e CKAN_KEY
- [Configurar proxy para instalar pacotes python](https://leifengblog.net/blog/how-to-use-pip-behind-a-proxy/)
