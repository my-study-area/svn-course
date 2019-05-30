# SVN Course
Curso sobre [Gerência de Configuração de Software com Subversion Parte 1 disponível](https://www.udemy.com/gerencia-de-configuracao-de-software-com-subversion-1/)

## Docker
- inicie o container
```sh
docker-compose up -d
```
- acesse o terminal do bash do container
```sh
docker-compose run --rm web bash
```


## 8. Operações Básicas de Controle de Versão com Subversion
- cria o repositório
```sh
svnadmin create projeto-x
```
- cria uma cópia do repositório:
```sh
svn checkout file:///home/adriano/Downloads/teste/svn-repositorios/projeto-x
cd projeto-x
```
- visualiza as informações do repositório:
```sh
svn info
```
- cria a estrutura padrão do svn:
```sh
svn mkdir trunk branches tags
```
- consolida as alterações (commit):
```sh
svn commit -m "'Criação da estrutura inicial trunk/branches/tags"
```
- recrie o projeto a partir do trunk:
```sh
cd ..
rm -rf projeto-x
git checkout file:///home/adriano/Downloads/teste/svn-repositorios/projeto-x/trunk
cd projeto-x
```
- verifica o estado do trabalho:
```sh
svn status
```
os estatus são:
- M - Modificado
- A - Adicionado ao projeto
- ? - Não rastreado
- I - Ignorado
- D - Removido
- ! Desaparecido
- C - Em connflito
- L - Travado para edição

- adiciona os arquivos não rastreados ao controle de versão:
```sh
svn add *
```
- verifica as alterações no arquivo modificado:
```sh
svn diff
```
- remove um arquivo
```sh
svn rm asdf.txt
```
ou
```sh
svn del asdf.txt
```
- copia um arquivo já rastreado para outro arquivo/diretório
```sh
svn cp <orig> <dest>
```
- renomeia ou move um arquivo/diretório
```sh
svn mv <orig> <dest>
```

- atualiza a cópia de trabalho com uma revisão.
```sh
svn update
```
- mostra o histórico
```sh
svn log
```
## 11. Ignorando arquivos pelo Subversion
- `svn status --no-ignore`: Mostra o estado do diretório de trabalho incluindo os arquivos ignorados.
- `svn propset NOMEPROP VALORPROP CAMINHO`: define o valor de uma propriedade
- `svn proplist -v CAMINHO`: lista todas as propriedades e apresenta seus valores
- `svn propget NOMEPROP ALVO`: imprime o valor de uma propriedade
- `svn propdel NOMEPROP CAMINHO`: exclui uma propriedade
- `svn propedit NOMEPROP ALVO`: edita uma propriedade usando um editor externo
- `svn propset svn:global-ignores 'test.txt'`: ignora o arquivo test.txt no controle de versão
- `svn add --force .`: adicona somente os arquivos não ignorados para o controle de versão

```sh
svnadmin create /home/adriano/Downloads/teste/svn-repositorios/projeto-2

svn mkdir file:///home/adriano/Downloads/teste/svn-repositorios/projeto-2/{trunk,branches,tags} \
-m 'Criação da estrutura padrão de diretórios'

svn checkout file:///home/adriano/Downloads/teste/svn-repositorios/projeto-2/trunk projeto-2

cd projeto-2/

mkdir -p static/{js,css,images} templates src .env build

touch README.txt README.txt.swp \
src/run.py src/run.py~ \
static/css/slides.styl static/css/slides.css \
static/images/fundo.jpg \
templates/home.html

svn propset svn:auto-props '*.sh = svn:executable=*
*.html = svn:eol-style=native
*.jpg = svn:needs-lock;svn:mime-type=image/jpeg
' .

svn status # vai listar o . como modificado
#  M      .
# ?       .env
# ?       README.txt
# ?       README.txt.swp
# ?       build
# ?       src
# ?       static
# ?       templates

#ignora os arquivos definidos
svn propset svn:global-ignores '*.swp
*~
*.css
.env
build
' .
# property 'svn:global-ignores' set on '.'

svn status --no-ignore
# M      .
# I       .env
# ?       README.txt
# I       README.txt.swp
# I       build
# ?       src
# ?       static
# ?       templates

svn add * #ação não esperada. Adicionou até os arquivos ignorados
# A         build
# A         README.txt
# A         README.txt.swp
# A         src
# A         src/run.py
# A         static
# A         static/images
# A  (bin)  static/images/fundo.jpg
# A         static/css
# A         static/css/slides.styl
# A         static/js
# A         templates
# A         templates/home.html

 svn revert -R * #desfaz ação anterior
# Reverted 'build'
# Reverted 'README.txt'
# Reverted 'README.txt.swp'
# Reverted 'src'
# Reverted 'src/run.py'
# Reverted 'static'
# Reverted 'static/css'
# Reverted 'static/css/slides.styl'
# Reverted 'static/images'
# Reverted 'static/images/fundo.jpg'
# Reverted 'static/js'
# Reverted 'templates'
# Reverted 'templates/home.html'

svn add --force . #respeita os arquivos ignorados
# A         README.txt
# A         templates
# A         templates/home.html
# A         src
# A         src/run.py
# A         static
# A         static/images
# A  (bin)  static/images/fundo.jpg
# A         static/css
# A         static/css/slides.styl
# A         static/js

svn status --no-ignore
# M      .
# I       .env
# A       README.txt
# I       README.txt.swp
# I       build
# A       src
# A       src/run.py
# I       src/run.py~
# A       static
# A       static/css
# I       static/css/slides.css
# A       static/css/slides.styl
# A       static/images
# A       static/images/fundo.jpg
# A       static/js
# A       templates
# A       templates/home.html

svn proplist -v . #mostra as propriedades e valores
# Properties on '.':
#   svn:auto-props
#     *.sh = svn:executable=*
#     *.html = svn:eol-style=native
#     *.jpg = svn:needs-lock;svn:mime-type=image/jpeg

#   svn:global-ignores
#     *.swp
#     *~
#     *.css
#     .env
#     build

svn proplist -v static/images/fundo.jpg #exibe as propridades da imagem
# Properties on 'static/images/fundo.jpg':
#   svn:mime-type
#     image/jpeg
#   svn:needs-lock
#     *

svn proplist -v README.txt #não retorna nada porque não possui propriedades

svn commit -m  'resolve #1 | arquivos filtrados e propriedades configuradas' #consolida as alterações
# Sending        .
# Adding         README.txt
# Adding         src
# Adding         src/run.py
# Adding         static
# Adding         static/css
# Adding         static/css/slides.styl
# Adding         static/images
# Adding  (bin)  static/images/fundo.jpg
# Adding         static/js
# Adding         templates
# Adding         templates/home.html
# Transmitting file data .....done
# Committing transaction...
# Committed revision 2.
```
## 12. Revertendo alterações pelo subversion
### antes de consolidar   
- reverte os arquivos requirements.txt LICENSE para o estado da consolidação
```sh
svn revert requirements.txt LICENSE
```
- reverte todos os arquivos modificados
```sh
svn revert -R .
```
- ação de cp e mv do sistema operacional não podem ser revertidas, será necessário desfazer as alterações pelo sistema operacional
- somente as exclusões podem ser consertas pelo comando `svn rm`
### Depois de compartilhado
#### Revertendo modificação de arquivo após a consolidação
- mostra os logs com alterações no arquivo LICENSE
```sh
svn log LICENSE
```
Saída do comando:
```sh
------------------------------------------------------------------------
r4 | adriano | 2019-05-27 22:29:13 -0300 (Mon, 27 May 2019) | 1 line

fase #3 | remoção de requirements.txt e mudança na licença
------------------------------------------------------------------------
r2 | adriano | 2019-05-21 20:24:26 -0300 (Tue, 21 May 2019) | 1 line

resolve #1 | configuração que servirá como base para os próximos passos
------------------------------------------------------------------------
```
- mostra o conteúdo do arquivo na revisão 3
```sh
svn cat -r 3 LICENSE
```
- outra forma de exibir o conteúdo do arquivo, mas através do caminho do respositório
```sh
svn cat file:///home/adriano/Downloads/teste/svn-repositorios/revertendo-alteracoes/trunk/LICENSE@3
```
- mostra o conteúdo do arquivo com um caminho relativo
```sh
svn cat ^/trunk/LICENSE@3
```
- restaura um arquivo pelo caminho relativo
```sh
svn cat ^/trunk/LICENSE@3 > LICENSE
```
#### Revertendo exclusão de arquivo após a consolidação
- `svn log nomedoarquivo.txt` não funciona para arquivos deletados
- `svn log -v` exibe o log so svn de forma verbosa e `grep -B 5 requirements.txt` filtra a saída do primeiro comando mostrando 5 linhas antes de encontrar a palavra `requirements.txt`
```sh
svn log -v | grep -B 5 requirements.txt
```
- melhorando a consulta podemos retornar somente a saída de arquivos deletados:
```sh
svn log -v | grep -B 5 'D.*requirements.txt'
```
Alterantiva 1: não mantém o histórico do arquivo deletado
```sh
svn cat ^/trunk/requirements.txt@3 > requirements.txt
```
Alternativa 2: mantém o histórico do arquivo
```sh
svn cp ^/trunk/requirements.txt@3 .
```
