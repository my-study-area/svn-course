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

