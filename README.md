# Aprendizados sobre Git
###### Estudos feitos no curso [Git e Github Essencial para o Desenvolvedor](https://www.udemy.com/course/curso-de-git-e-github-essencial/)

<details><summary><strong>Sincronizando repo local com o remoto</strong></summary>

- Crie o repositório no próprio Github, é bem fácil. Depois na sua máquina, entre na sua pasta de repositórios. No terminal digite:  
  **`git clone UrlDoRepo`**  
  **`cd Repo`**  
  **`git config user.name ""`**  
  **`git config user.email ""`**  
  **Crie algum arquivo ou altere um existente**  
  **`git add .`**  
  **`git commit -m "Update"`**  
  **`git push -u origin main`**  
  
  *(o processo abaixo tem que ser feito duas vezes para não precisar mais)*
  
  **Pedirá username e password (ou token se você tem 2FA)**
  
  *(só rode o comando abaixo se estiver em máquina pessoal)*
  
  **`git config credential.helper store`** pra guardar as credenciais, senão vai ter que colocar login e senha em todo push.
    
  *(sempre que mudar algo como username ou nome do repo, entre na pasta .git e faça as alterações no arquivo config, de cada repo)*

</details>

<details><summary>Ciclo de vida dos arquivos</summary>

- **Untracked:** estados em que todos arquivos iniciam. Quando não está rastreado, sincronizado no repo local, no Git.
- **Tracked:** quando o arquivo está rastreado pelo Git, está sob o controle de versionamento.
- **Modified:** quando modifica um arquivo já rastreado. O Git te avisa que precisa atualizar o rastreamento.
- **Staged:** quando o arquivo está pronto pro commit.

</details>

<details><summary>Comandos Básicos</summary>

- **`history -c`** --> Apagar histórico do terminal git/linux.
  - Apagar de forma mais completa: **`cat /dev/null > ~/.bash_history && history -c`**
- **`git init`** --> Inicializar um repositório.

- **`git status`** --> Checar o estado dos arquivos do repo.

- **`.gitignore`** --> Bem auto explicativo, é um arquivo em que você coloca arquivos/diretórios/etc, que você quer que o git ignore. Normalmente usado pra banco de dados, lógica de negócios, autenticações, etc.
  - Para arquivos, coloque o arquivo e extensão, exemplo **`video.mp4`** **`db.sqlite`** etc
  - Para ignorar vários arquivos com a mesma extensão, use **\*** e a extensão, exemplo **`*.sqlite3`**
  - Para diretórios, coloque **\*\*** e o nome do diretório, exemplo **`**videos`** **`**database`**

- **`git config user.name ""`** --> configurar seu nome de usuário.

- **`git config user.email ""`** --> configurar email do usuário.
  - Se estiver numa máquina pessoal, de uso exclusivo, utilize **`--global`** depois do **`config`** para que todos projetos comecem com essa configuração padrão.

- **`git add`** seguido do nome e extensão do arquivo, para adicionar arquivos ao monitoramento do git. **Também** é usado quando você modifica um arquivo.

- **`git add .`** --> diz pro git tanto pra adicionar arquivos novos pro monitoramento, quanto pra monitorar os modificados.

- **`git mv arquivo1.extensao arquivo2.extensao`** --> renomeia arquivos. Serve pra diretórios também. Certifique-se de estar no dir correto, e usar **`git mv ./pasta1/ ./pasta2/`**
  > Por que fazer isso pelo git e não pelo terminal normal? Porque quando você faz isso, na verdade o arquivo anterior é apagado, e é criado um nome arquivo, com mesmo conteúdo mas nome diferente. Você tem que adicionar novamente o arquivo, com o novo nome, ao rastreio do Git, e também tem que adicionar o arquivo deletado (??wtf??) com o nome antigo.

  > Renomeando pelo próprio git, o arquivo só muda o nome mesmo, continua rastreado, pronto pro commit. Muito menos dor de cabeça.

- **`git rm arquivo.extensao`** --> deletar arquivo. **`git rm -rf pasta/`** --> deletar diretório
  > Mas preste atenção, só pode excluir um diretório ou arquivo que já esteja sendo tracked pelo Git, do contrário vai dar erro, pois pra ele "não existe". Ah, e diretórios vazios não são sequer enxergados pelo Git, ele nem dá algum aviso. E portanto não dá pra remover, são untracked.

- **`git diff`** vem de difference, mostra as diferenças de um estado pro outro, de um commit pro que virá.
  - Você tem que adicionar algo amais, exemplo **`git diff --staged`** para verificar diferença do anterior pro atual.
  - **`git diff hash`** --> verificar a diferença com um commit especifico.
  - **`git diff hash..hash`** para ver a diferença de um commit **até** o outro.

</details>

<details><summary>Commit</summary>

- Um commit é tipo um snapshot do arquivo/algoritmo que está desenvolvendo. É um "okay" pro repo local e informa que o arquivo está pronto para ir pro repo remoto.
  - **`git commit -m ""`** onde **-m** significa a mensagem que aparecerá no commit.

- Sempre que você fizer um commit, irá gerar um hash id, um identificador, exemplo **`[main 9da4dd5]`**

- Quando esquecer de mandar certas mudanças pro mesmo commit, ou esquecer arquivos, etc, **antes do push**, você pode usar **`git commit --amend -m "mensagem"`** para fazer essas adições ao último commit.

- Quando você adiciona um arquivo, deixa ele tracked, mas se arrepende, quer remover do track do Git, **`git restore --staged <file>`**

</details>

<details><summary>Log/Histórico</summary>

- **`git log`** mostra o log de commits, autor, email, timestamp e hash.
  - Quando tem muitos commits, ele reduz a visão no terminal.
  - Você pode usar **`/`** e digitar conteúdo da mensagem do commit para procurar. **`b`** para voltar. **`q`** para sair.
  - (se você quiser fazer com que ele pare de reduzir o log, use **`git config core.pager cat`**
  - (se quiser que volte ao normal, use **`git config core.pager less`**
  - (Lembrando que são configs locais, se quiser de forma global utilize **`--global`** depois do **`config`**)

- Você pode usar **-** e um número, para informar os últimos commits que quer ver, Ex: **`git log -2`**

- **`git log --oneline`** mostra as informações de forma reduzida, o hash e mensagem. Inclusive pode combinar isso com o de cima.

- Você pode procurar por datas, exemplo: **`git log --before="2020-12-13" | git log --after="2020-12-10" | git log --after="2020-12-01" --before="2020-12-12" | git log --since="7 days ago"` |** (Lembrando que também pode mesclar com o ante anterior).

- Pode pesquisar pelo autor do commit **`git log --author="Gustavo"`**

</details>

<details><summary>Checkout</summary>

> Através do hash id, conseguimos desafazer mudanças. Lembre-se que um commit é um snapshot, uma foto do projeto, você pode entrar naquela foto e voltar pro momento, igual Life is Strange.

- **`git checkout`** e o hash id, exemplo **`0e1b5fa`**

- Se você só quiser checar algo e voltar pro futuro, ou se arrepender, pode usar **`git checkout main`**

- Quando se arrepender de uma mudança em um arquivo, tiver feito merda, **antes dele estar add, monitorado**, pode usar **`git checkout <file>`** que o arquivo voltará ao estado do último commit feito.

- Pra fazer isso com todo projeto: **`git reset HEAD --hard`**

- Para fazer isso, depois de ter commitado, (você irá voltar todo projeto pro último commit) **`git reset HEAD^ --hard`**

- Para voltar todos arquivos pro estado original, do último commit, antes de estarem tracked, **`git checkout -- .`**

- Para fazer isso com apenas um arquivo **`git checkout -- <filename>`**

- Para fazer isso depois do arquivos estarem tracked: **`git checkout HEAD -- .`**

- Para fazer isso com apenas um arquivo **`git checkout HEAD -- <filename>`**

</details>

<details><summary>Revert e Reset</summary>

- **Revert**: não desfaz um commit, ele reverte o que foi feito e criando um novo commit. Reverte. **`git revert <HashDoCommit>`**
  - Não esqueça de dar o **push** pro commit ir pro bare.

- **Reset:** remove commits. **`git reset HEAD~1`**
  - **`git push -f -u origin main`**

</details>

<details><summary>Branchs</summary>

> Quando você cria um projeto no git, você tem seu **branch main**, que seria o **tronco** da árvore. É perigoso ficar commitando no tronco, pois se fizer algo errado, vai estragar toda árvore. Por isso você tem o conceito de **branchs secundárias**, que seriam os **galhos**, as **ramificações**. Então você está lá desenvolvendo certa **feature** do projeto, se ela der errado, você simplesmente joga o galho fora, corta ele. Mas se der certo, você faz um **merge**, **junta** o galho ao tronco, junta a branch secundária com a feature para a branch main.

- **`git branch`** retorna quantas branchs existem e em qual branch você está (em verde e com um asterisco *) 

- Para criar uma branch é bem simples **`git branch NomeDaBranch`**

- Alternar entre branchs --> **`git checkout NomeDaBranch`**
  - (Se você quiser economizar tempo, pode criar e já alternar pra branch, com um comando só: **`git checkout -b NomeDaBranch`**)

- Excluir uma branch --> **`git branch -d NomeDaBranch`** 
  - Se a branch que vai ser excluída não foi fundida com outra em algum momento, o git vai perguntar se quer mesmo excluir, aí tem que rodar o mesmo comando, mas em caps o **`-D`**

- Pra dar um **merge** você alterna pra branch que vai *absorver a outra* (normalmente a main) e digita **`git merge NomeDaBranchAbsorvida`**
  - (Lembrando que após o merge, a branch absorvida não desaparece, ela continua viva e independente). Ah, e quando tal branch recebe o merge, ela absorve também os commit feitos, todo log etc

- **Rebase** faz quase a mesma coisa que **merge**, mas deixa os commits em ordem, reoorganiza a ordem de todos commits do projeto. **`git rebase NomeDaBranch`**
  - Não é super indicado, principalmente em pair programming e em empresa. É até legal para projetos pessoais, mas melhor não usar.

</details>

<details><summary>Clone, Push, Fetch, Pull e Tag</summary>

- Pra clonar um repositório --> **`git clone urlDoRepo .`** (o ponto indica pra clonar dentro do repo que está)
  - Depois de clonar, entre no repo e configure seu usuário.

- O **push** "empurra" pro repo remoto, o bare. **`git push -u origin main`** --> envia seus commits pro repo central

- O **fetch** baixa os arquivos, mas sem trackear. **`git fetch`** aí depois tem que usar o git rebase pro arquivo organizar os arquivos e commits **`git rebase`**
  - Método menos utilizado.

- O **pull** faz isso acima em uma tacada só **`git pull origin main`** (vai abrir um editor de código, só digitar ^O + enter + ^X)

- A **tag** é um estado da aplicação, como se fosse um release, a versão. **`git tag versaoTal`**
  - Mas por enquanto isso só está no repo local. Para mandar pro repo remoto, para que todos users saibam da release **`git push origin versaoTal`**
  - Inclusive, você pode alternar para tags, para "dar uma olhada", igual faz em branchs. **`git checkout versaoTal`**
  - Você pode usar isso pra criar uma branch a partir de tal tag, tpo pra corrigir bugs de tal versão, etc. **`git switch -c <new-branch-name>`**

- **Bare repository**: Significa repositório central, remoto. Lembrando que o git é descentralizado, mas é comum que tenhamos um repositório central, ainda mais quando trabalhamos em equipe.

</details>

<details><summary>Issue, Fork e Pull Request</summary>

- **Issue:** quando uma pessoa acha um problema em um projeto seu, pode reportar uma **issue**. Você também pode fazer isso com os outros. Mas quando reportar uma issue, pesquise bem antes, pra não criar uma que já foi resolvida.
  - Dá pra fechar uma issue no commit, dentro da mensagem dele, no final coloque **`Closes #IssueID`**

- **Fork:** normalmente você forka um projeto pra resolver uns bugs ou melhorar e dar pull request, ou também quando quer criar algo novo com base naquele.

- **Pull request:** é uma requisição para que o owner aceite as alterações feitas no se fork para o bare. Você também pode passar no título do pull request **`Closes #IssueID`** para que além de aceitar, fechar uma issue dele.
  - É uma boa prática ao invés de dar um merge com pull request, você dar um fetch (lembrando que o fetch baixa mas sem fundir), pra testar se realmente está tudo certo.  **`git fetch origin pull/IdPullRequest/head:NomeDaBranch`**
  - Aí você olha o log, verifica o arquivo mexido, se está legal. E então vai no github e confirma o merge do pull request.

</details>

<details><summary>Gist</summary>

Pequenos trechos de códigos que você cria pra você mesmo ou outras pessoas. Snippets.

Para usar facilmente com frequência.

Permite o compartilhamento de pequenos trechos de código. Há também quem use o Gist para receber feedbacks daquele código específico. Também pode publicar parte do seu código e usar o plugin do Gist para mostrar seu código em sites, fóruns e outros locais. Para isso, só precisa publicar o código (depois de logar no GitHub) e clicar em “Show Embed” e ele lhe mostrará um código javascript para colar onde quiser. Onde você colar o javascript vai aparecer uma caixinha bonitinha com o trecho de código e um link para o seu Gist. Alterando seu Gist, todos os lugares onde você publicou seu código serão alterados ao mesmo tempo.

</details>
