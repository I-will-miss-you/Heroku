
# [Heroku](https://www.heroku.com/)

## O que é o Heroku
_Retirado de:_ [iMasters](https://imasters.com.br/box/ferramenta/heroku/) 
> Heroku é uma plataforma de serviço em nuvem (PaaS) suportando várias linguagens de programação. Heroku é de propriedade da Salesforce.com . Heroku, uma das primeiras plataformas de nuvem , já está em desenvolvimento desde junho de 2007, quando suportava apenas a linguagem de programação Ruby , mas, desde então, adicionou suporte para Java , Node.js , Scala , Clojure e Python e PHP. O sistema operacional de base é Debian ou, no mais recente, o Debian-based Ubuntu.

> Se você quer lançar uma aplicação Rails rapidamente, não existe melhor solução do que o Heroku. Para quem não conhece, o Heroku é um Paas (Platform as a Service) que roda sobre o Amazon EC2 (que é um IaaS ou Infrastructure as a Service). O Heroku automatiza a criação de uma nova máquina virtual e configura todo o ambiente para rodar Ruby.

> O Heroku usa uma unidade de máquina virtual chamada “Dyno”, a grosso modo, considere um Dyno como uma máquina virtual “pequena” com 4 cores e até 512Mb de RAM sem swap file e sem suporte a persistência de arquivos . Configurar um novo ambiente é simples, o próprio Heroku tem uma boa documentação ensinando como e recomendo ler antes de continuar.
Subir uma única dyno usando um banco de dados compartilhado PostgreSQL é de graça, o que é excelente para testar sua aplicação. Obviamente apenas um único dyno é pouco para qualquer aplicação séria lançada em produção para o público.

> O Heroku fornece “stacks” padrão que é o perfil pré-configurado de um dyno para uma determinada plataforma. Para Ruby e Rails a mais atual (na data de publicação deste post) é a Celadon Cedar, a anterior era a Badious Bamboo portanto se encontrar um tutorial qualquer de Heroku por aí, cheque sobre qual stack estamos falando, só use se for para Cedar.
Heroku tambem é uma hospedagem para aplicações construídas em Ruby on Rails, uma hospedagem bastante interessante e com um monte de add-ons para adicionar mais funcionalidades a aplicações.

## Heroku e Heroku toolbet

### Heroku toolbet
-  Instalação
> https://devcenter.heroku.com/articles/heroku-cli

- Login
```sh
heroku login
```

- Criar a app
```sh
heroku create Nome_da_Aplicação
```

- Fazer o Deploy
> Importante: https://devcenter.heroku.com/articles/getting-started-with-php#define-a-procfile

- Instalar o Papertr (gerência os Logs)
> No menu _elements_ selecionar o Add-on __Papertrail__
> https://elements.heroku.com/addons/papertrail

- Instalar a Data Base 
> Postgresql
> https://elements.heroku.com/addons/heroku-postgresql

- Configurando variáveis de ambiente da data base

- Criar as tabelas no Data Base
```
heroku run  php artisan migrate:refresh
```

- __Aceder a bash do projeto__
```
heroku run bash
```

- Gerar dados falsos para o banco de dados (Laravel)
```
composer update
php artisan db:seed
```

- Conetar o Heroku com o GitHub
    - Criar um novo repositorio no git
    - Adicionar o projeto ao repositorio
    ```
    git remote add upstream "endereço do repositorio"
    git push upstream master
    ```
    - No Heroku dentro do projeto: » Deploy » selecionar GitHub » Connect to GitHub » idicar o repositorio a que se deve conetar » Connect
    - Ativar o "Automatic Deploy"
    - Deploy a GitHub brach -> master

- Criar uma pipeline   
    - fazer um fork do projeto do GitHub
    - No _Heroku_ » Deploy » New Pipeline
    - » Connection to GitHub - Selecionar o projeto principal » connect
    - Fazer o fork da app
    ```
    heroku fork --from nome_da_aplicação --to nome_da_aplicação-staging
    ```
    - atualizar as credenciais da Base de Dados da app staging
    - » STAGING - add o projeto nome_da_aplicação-staging

- Promovendo de Staging para Prodution
    - No pc
    ```
    # staging ou outro nome qualquer
    git remote add staging https://git.heroku.com/nome_app_staging.git

    git push staging master
    ``` 
    - No Heroku ... » nome_do_projeto-stagig » STAGING » Promote to prodution...

- Criar ambiente de desenvolvimento
    - _Adaptando conexão com banco de dados
    - ... RIVIEW APPS » Enable Review Apps...
    - Select a app staging
    - Na raiz projeto criar um arquivo app.json » copia o JSON para dentro do arquivo
    - Commit to repo
    - Promover para produção
    - Selecionar as duas opções » enable 

> projeto_em_produção » Disable Automatic Deploys   
> projeto_staging » Enable Automatic Deploys    
