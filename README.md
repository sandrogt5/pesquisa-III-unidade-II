pesquisa-III-unidade-II
=====================

Mercurial é uma ferramenta multi-plataforma de controle de versão distribuído para desenvolvedor de software.
O sistema é implementado principalmente em Python, porém o utilitário binário diff foi escrito em C. Mercurial foi inicialmente escrito para rodar sobre Linux,
mas foi portado para Windows, Mac OS X, e a maioria dos outros sistemas UNIX. Mercurial é principalmente um programa de linha de comando.
Todas operações do Mercurial são chamadas através de palavras chave de opções para o programa controlador hg,
uma referência para o símbolo químico do elemento Mercúrio.

Mercurial funçoes basicas:

Criando um Repositório

Crie um repositório com o comando hg init:

alice$ ls
alice$ hg init example
alice$ ls
example

Criando um Changeset

Vamos criar um arquivo e perguntar ao Mercurial sobre o estado do repositório:

alice$ echo "Hello World" > hello.txt
alice$ hg status
? hello.txt

A resposta mostra que hello.txt é desconhecido pelo Mercurial, isto é, ainda não é rastreado.
Nós podemos adicionar o arquivo:

alice$ hg add hello.txt
alice$ hg status
A hello.txt

isto muda o estado para “A” de “Adicionado”. O arquivo ainda não está armazenado no histórico do repositório 
Faremos isso através da consolidação (commit):


alice$ hg commit -m "First version of hello"

Nenhuma mensagem indica sucesso — O Mercurial não interrompe o fluxo de trabalho a não ser que seja necessário.

Inspecionando o Histórico

Pode-se ver o changeset recem criado com o comando hg log:

alice$ hg log
changeset:   0:d312da7770f4
tag:         tip
user:        Alice <alice@example.net>
date:        Wed Mar 10 20:10:05 2010 +0000
summary:     First version of hello

Sucesso, você fez uma consolidação com o Mercurial! Vamos modificar o arquivo e pedir ao Mercurial para mostrar
como os arquivos na cópia de trabalho diferem da última revisão:

alice$ echo "Goodbye!" > hello.txt
alice$ hg diff
diff -r d312da7770f4 hello.txt
--- a/hello.txt Wed Mar 10 20:10:05 2010 +0000
+++ b/hello.txt Mon Mar 10 20:10:10 2010 +0000
@@ -1,1 +1,1 @@
-Hello World
+Goodbye!

Podemos mudar de ideia e reverter o arquivo de volta ao estado em que ele se encontrava antes:

alice$ hg revert hello.txt
alice$ cat hello.txt
Hello World

Vamos fazer outra mudança no nosso arquivo:

alice$ echo >> hello.txt
alice$ echo "by Alice." >> hello.txt
alice$ hg commit -m "Added author." 

Podemos pedir para o Mercurial para mostrar as anotações sobre o arquivo, isto é, mostrar quando cada linha foi modificada pela última vez:

alice$ hg annotate hello.txt
0: Hello World
1:
1: by Alice.

Muito bem, o Mercurial nos diz que a primeira linha é da revisão 0 e que as duas últimas linhas são da revisão 1.
O TortoiseHg oferece uma interface muito boa dessa ferramenta de notas. Veja thg annotate.

As anotações dos arquivos são uma ajuda inestimável para consertar defeitos: após achar o defeito, 
você pode usar hg annotate para descobrir quando a linha problemática foi introduzida no arquivo.
Em seguida, usar hg log para recuperar a mensagem de consolidação associada, 
que deve explicar o que a pessoa responsável estava tentando fazer quando mudou a linha:

alice$ hg log -r 1
changeset:   1:2e982bdc137f
tag:         tip
user:        Alice <alice@example.net>
date:        Wed Mar 10 20:15:00 2010 +0000
summary:     Added author.

Os dois changesets no nosso repositório estão em sequência. Isto é melhor ilustrado com a ajuda da extensão graphlog.
Para manter o núcleo do Mercurial enxuto, muitas funcionalidades são delegadas a extensões. Muitas das melhores extensões
são entregues com o Mercurial e são fáceis de serem habilitadas. Para habilitar a extensão graphlog, Alice simplesmente adiciona:

[extensions]
graphlog =

ao arquivo de configuração dela. Ela pode, então, executar hg help graphlog para aprender um pouco mais sobre a extensão.
Esta extensão é particularmente simples, ela apenas fornece a Alice um novo comando:

alice$ hg glog
@  changeset:   1:2e982bdc137f
|  tag:         tip
|  user:        Alice <alice@example.net>
|  date:        Wed Mar 10 20:15:00 2010 +0000
|  summary:     Added author.
|
o  changeset:   0:d312da7770f4
   user:        Alice <alice@example.net>
   date:        Wed Mar 10 20:10:05 2010 +0000
   summary:     First version of hello
Isto mostra como o changeset 2e982bdc137f` é um filho do changeset ``d312da7770f4. O mesmo pode ser visto pelo thg log:
Em seguida, usar hg log para recuperar a mensagem de consolidação associada,Os arquivos na cópia de trabalho são sempre
sincronizados com um changeset particular. No thg log, isto é mostrado pelo círculo no grafo de changesets. No resultado
apresentado pelo hg glog, a revisão-pai do diretório de trabalho é destacado pelo símbolo @.
