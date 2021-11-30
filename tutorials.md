# Tutoriais breves durante a jornada Decola Tech 2ª ed.

## Fluxo de utilização básico Git e Github

### 1. **Instalar o Git na máquina de acordo com o SO.** 
[Download Git](https://git-scm.com/downloads)

***Dica 1:***

No caso do **Windows** é interessante marcar a opção de instalar o **Gitbash** que emula comandos do terminal Linux.

***Dica 2:***

O **Windows Terminal** permite algumas funcionalidades como dividir o terminal em guias ou mesmo em abas entre outros recursos em relação ao terminal **Prompt de Comando (cmd)** e **Windows Power Shell**, sendo possível integrar o Gitbash ao Windows Terminal uma vez o mesmo já instalado, basta que na hora de instalação do Git seja marcada a opção de integração. (Isso pode também pode ser feito após a instalação).

[Windows Terminal](https://docs.microsoft.com/pt-br/windows/terminal/)

[Personalizando Windows Terminal](https://docs.microsoft.com/pt-br/windows/terminal/tutorials/custom-prompt-setup)

***Dica 3:***

Existe um **gerenciador de pacotes para Windows** chamado [Chocolatey](https://chocolatey.org/), por onde também é possível **instalar o Git via linha de comando**.

### 2. Realizar configurações iniciais básicas:

Agora que você tem o Git em seu sistema, você deve fazer algumas coisas para personalizar o ambiente Git. Você fará isso apenas uma vez por computador e o efeito se manterá após atualizações.

O Git vem com uma ferramenta chamada **git config** que permite ver e atribuir variáveis de configuração que controlam todos os aspectos de como o Git aparece e opera.

**Sua Identidade**

A primeira coisa que você deve fazer ao instalar Git é configurar seu nome de usuário e endereço de e-mail. Isto é importante porque cada commit usa esta informação, e ela é carimbada de forma imutável nos commits que você começa a criar:

```bash
$ git config --global user.name "Fulano de Tal"

$ git config --global user.email "fulanodetal@exemplo.br"
```

Reiterando, você precisará fazer isso somente uma vez se tiver usado a opção **--global**, porque então o Git usará esta informação para qualquer coisa que você fizer naquele sistema. Se você quiser substituir essa informação com nome diferente para um projeto específico, você pode rodar o comando sem a opção --global dentro daquele projeto.

Muitas [ferramentas GUI](https://git-scm.com/downloads/guis) o ajudarão com isso quando forem usadas pela primeira vez.


**Testando Suas Configurações**

Se você quiser testar as suas configurações, você pode usar o comando **git config --list**

```bash
$ git config --list
user.name=Fulano de Tal
user.email=fulanodetal@exemplo.br
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
 ```
 Você pode também testar o que Git tem em uma chave específica digitando **git config \<key>:**

```bash
$ git config user.name
Fulano de Tal
```

### Estabelecer uma conexão do Git com a conta de usuário do Github:

Isso não pode mais ser feito através de credenciais comuns como método de acesso para os desenvolvedores.

Os meios disponíveis mais comuns estão a autenticação usando:

- **Chave SSH**
- **Chave GPG**
- **Token**

Neste caso, configurarei uma chave SSH obtendo acesso sem mais nenhuma solicitação de credenciais na máquina que foi configurada para o acesso.

Partindo do presuposto que não tenham sido geradas chaves anteriores que se queira usar precisamos gerar as chaves SSH e adicionar a chave Pública nas configurações do Github.

#### Gerando um novo par de chaves SSH:

1. Abra o terminal e digite:

```bash
$ ssh-keygen -t rsa -b 4096 -C "seuemailaqui@example.com"
```
2. Será perguntado onde será salvo o arquivo com o par de chaves, e se deseja entrar com uma senha para sua chave, basta apertar enter e a configuração padrão será aplicada: Sem senha para a chave e por padrão os arquivos de chave serão salvos na home do seu usuário. Você terá um arquivo com a extensão ".pub" é onde está sua chave pública. Abra o arquivo e copie todo o seu conteúdo. (Pode abrir em qualquer editor de texto, inclusive notepad, ou pode usar os comandos abaixo para obter a partir do terminal:)

- **No Windows:** ```clip < ~/.ssh/id_rsa.pub``` (Automaticamente o conteúdo da sua chave pública já será copiado para a área de transferência, ou seja, está no seu "Control + V".)

- **No Linux:** ```$ cat ~/.ssh/id_rsa.pub```
(O conteúdo da chave pública aparecerá no terminal para ser selecionado e copiado.)

#### Adicionar chave privada no ssh-agent
O ssh-agent é um gerenciador de chaves ssh. Para que a conexão funcione, devemos adicionar a chave privada nesse gerenciador. Para isso vamos executar os códigos:

**Rodar o ssh-agent:** ```bash eval $(ssh-agent -s)```

**Incluir a chave privada:** ```$ ssh-add ~/.ssh/id_rsa```


#### Adicionar chave no Github

- Abra o Github e vá no ícone de perfil > Settings, no canto superior direito.
- Na barra lateral de configurações do usuário, clique em "SSH and GPG keys".
- Clique no botão "New SSH key"
No campo "Título", adicione um rótulo descritivo para a nova chave. Por exemplo, se estiver usando seu computador pessoal, você pode chamar essa chave de "Computador pessoal".
- **Cole a chave pública** (_id_rsa.pub_) que está na área de transferência no campo "Chave".
- Clique em "Add SSH key" e pronto!

#### Testando a conexão SSH

Executar o seguinte comando:

```$ ssh -T git@github.com```

Aguardar as mensagens. Digitar "yes" para continuar.
Verifique se a mensagem resultante contém seu nome de usuário e o sucesso da sua autenticação.

### Criar e inicializar o repositório:

Você pode obter um projeto Git utilizando duas formas principais. A primeira faz uso de um projeto ou diretório existente e o importa para o Git. A segunda clona um repositório Git existente a partir de outro servidor.

#### Primeira forma - Criando o repositório localmente:

1. Crie uma pasta para seu projeto e acesse a mesma
2. Inicialize o repositório Git no através do comando: ```$ git init```

_Isso cria um novo subdiretório chamado .git que contem todos os arquivos necessários de seu repositório — um esqueleto de repositório Git. Neste ponto, nada em seu projeto é monitorado._

3. Utilize o comando  ```$ git add``` para adicionar os arquivos que deseja rastrear. **ex.:** ```$ git add arquivo.txt```

_O comando git add pode ser usado para adicionar arquivos ao índice. **Assim esses arquivos passam a ser monitorados, a partir de agora o Git vai "observar" qualquer alteração dentro do repositório nesses arquivos adicionados.**_

Passando do estado **Não rastreado** (_Untracked_) para **rastreado** (_Tracked_)

4. Utilize o comando ```$ git commit``` para criar uma versão do seu código.

_O comando git commit é usado para confirmar as alterações nos arquivos e criar uma uma versão do seu programa. Como uma grande "foto" do estado atual._

```bash
$ git commit –m “coloque sua mensagem sobre o código aqui”
```

5. Utilize o comando ```git status``` para ver o estado dos arquivos do repositório:

_O comando git status exibe a lista de arquivos alterados juntamente com os arquivos que ainda não foram adicionados ou confirmados._

```$ git status```

### Trabalhando com os repositórios Remotos:

Agora que já criamos nosso repositório git e foi criada nossa versão, é hora de ***"empurrar"*** (_push_), colocar no servidor remoto a nossa versão criada, que até então só está em nossa máquina pessoal local.

1. Criamos o repositório no Github e copiamos o endereço SSH desse repositório criado.

2. Adicionamos o repositório remoto através do endereço do mesmo, nomeando com um apelido. Por padrão é comum usar o apelido: _"origin"_.

Para adicionar um novo repositório Git remoto como um nome curto que você pode referenciar facilmente, execute:

```bash
$ git remote add <apelido> <url>
```

3. Para enviar definitivamente para o servidor remoto executamos o comando:

```bash
$ git push origin master
```
Onde **_master_** é o antigo nome da branch padrão, hoje migrando para **_main_**.


## Referências utilizadas:

[Começando - Instalando o Git - Oficial](https://git-scm.com/book/pt-br/v2/Come%C3%A7ando-Instalando-o-Git)


[Verificar se há chaves SSH - Oficial](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

[Gerar uma nova chave SSH e adicioná-la ao ssh-agent - Oficial](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

[Como configurar o SSH no Github? - Adriana Lima Shikasho](https://dev.to/dxwebster/como-conectar-ao-github-com-chaves-ssh-1i41)

[Criando chave SSH - Git e Github para Iniciantes - Willian Justen](https://www.youtube.com/watch?v=7YVQLZp1jb0)














