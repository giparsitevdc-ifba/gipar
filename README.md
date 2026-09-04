# Desenvolvimento de sistemas e sites com WordPress

Este repositório reúne orientações para criar, configurar e publicar sistemas e sites utilizando WordPress, PHP e as ferramentas do ecossistema web.

## 1. Defina o projeto

Antes de iniciar o código:

- descreva o objetivo do sistema ou site;
- identifique os perfis de usuário e suas permissões;
- liste as páginas, funcionalidades e integrações necessárias;
- defina requisitos de acessibilidade, desempenho, segurança e responsividade;
- escolha um domínio, ambiente de hospedagem e estratégia de backup.

Para sistemas maiores, registre também os fluxos principais, o modelo de dados e os critérios de aceite de cada funcionalidade.

## 2. Prepare o ambiente local

Instale:

- PHP em uma versão suportada pelo WordPress;
- Composer;
- MySQL ou MariaDB;
- Node.js e npm, quando o tema ou plugin tiver ferramentas de frontend;
- Git;
- um ambiente local como DDEV, Docker, Local ou XAMPP.

### WordPress com Docker Compose

O arquivo `compose.yaml` inicia o WordPress e o MySQL com volumes persistentes. Para começar:

```bash
cp .env.example .env
# Edite .env e altere as duas senhas
docker compose config
docker compose up -d
```

Acesse `http://localhost:8080` ou a porta definida por `WORDPRESS_PORT`. Para acompanhar os logs e desligar os serviços:

```bash
docker compose logs -f wordpress
docker compose down
```

Os dados ficam nos volumes `wordpress_data` e `db_data`. Para remover também esses dados, use `docker compose down -v`.

### Exemplo usando DDEV:

```bash
ddev config --project-type=wordpress --docroot=web
ddev start
ddev wp core download
ddev wp core install \
  --url=https://wordpress.ddev.site \
  --title="Meu projeto" \
  --admin_user=admin \
  --admin_email=admin@example.com
```

Use senhas reais apenas localmente e nunca versione credenciais no repositório.

## 3. Organize o projeto

Uma estrutura recomendada para um projeto WordPress versionado é:

```text
.
├── README.md
├── composer.json
├── package.json
├── web/
│   ├── index.php
│   ├── wp-config.php
│   └── wp-content/
│       ├── mu-plugins/
│       ├── plugins/
│       ├── themes/
│       └── uploads/
└── .gitignore
```

Mantenha no Git o código próprio do projeto, como temas, plugins, configurações de build e documentação. Dependências podem ser instaladas com Composer ou npm durante a preparação do ambiente.

## 4. Crie sites institucionais

Para um site institucional:

1. escolha um tema base mantido e compatível com a versão do WordPress;
2. crie um tema filho ou um tema próprio para personalizações;
3. configure páginas, menus, identidade visual e áreas de conteúdo;
4. utilize campos personalizados quando o conteúdo exigir uma estrutura consistente;
5. valide o layout em telas pequenas, médias e grandes;
6. configure SEO, sitemap, formulário de contato, analytics e política de privacidade;
7. teste navegação por teclado, contraste, textos alternativos e leitores de tela.

Evite editar diretamente temas ou plugins de terceiros, pois atualizações podem apagar as alterações.

## 5. Crie sistemas com WordPress

Para transformar o WordPress em uma aplicação:

- modele entidades e relacionamentos antes de definir onde os dados serão armazenados;
- use tipos de post, taxonomias e metadados para extensões compatíveis com o núcleo;
- use tabelas próprias apenas quando o volume, a consulta ou a natureza dos dados justificar;
- implemente regras de negócio em plugins, mantendo o tema responsável principalmente pela apresentação;
- valide, sanitize e escape todos os dados recebidos ou exibidos;
- proteja ações administrativas com nonces e verificação de capacidades;
- use a REST API ou APIs internas com autenticação e autorização explícitas;
- registre erros sem expor informações sensíveis ao usuário final;
- escreva testes para regras de negócio e fluxos críticos.

### Exemplo de fluxo de desenvolvimento de um plugin

1. crie um plugin com cabeçalho e namespace próprios;
2. registre hooks somente quando necessário;
3. defina as capacidades exigidas por cada ação;
4. adicione validação e sanitização na entrada;
5. use consultas preparadas para valores variáveis;
6. escape a saída conforme o contexto (`esc_html`, `esc_attr`, `esc_url` ou equivalente);
7. documente a configuração e os comandos de instalação;
8. teste ativação, desativação, atualização e remoção.

## 6. Dependências e qualidade

Use Composer para dependências PHP e npm para ferramentas JavaScript ou CSS. Antes de enviar alterações:

```bash
composer install
composer validate
composer audit
npm install
npm run build
```

Adapte os comandos ao conteúdo de `composer.json` e `package.json`. Quando disponíveis, execute também testes automatizados, análise estática e lint:

```bash
vendor/bin/phpunit
vendor/bin/phpstan analyse
vendor/bin/phpcs
```

## 7. Segurança

- mantenha WordPress, PHP, temas e plugins atualizados;
- instale extensões apenas de fontes confiáveis;
- use HTTPS em todos os ambientes públicos;
- utilize o princípio do menor privilégio;
- não versione `wp-config.php`, arquivos `.env`, chaves ou senhas;
- desabilite o modo de debug em produção e proteja os logs;
- faça backups testados do banco e dos arquivos enviados;
- limite tentativas de login e habilite autenticação multifator quando possível;
- remova plugins, temas e contas que não são utilizados.

## 8. Publicação

Antes do deploy:

- valide o ambiente de produção e as versões de PHP e banco;
- configure variáveis de ambiente e chaves de autenticação;
- gere dependências com modo de produção;
- faça backup antes de migrações;
- teste links permanentes, formulários, uploads, e-mails, cache e integrações;
- acompanhe logs e métricas após a publicação;
- documente como reverter a versão publicada.

Nunca faça alterações diretamente em produção sem registro e sem uma forma de recuperação.

## 9. Fluxo Git recomendado

Use branches curtas e commits objetivos:

```bash
git checkout -b feature/nome-da-funcionalidade
git add .
git commit -m "Adiciona funcionalidade X"
git push -u origin feature/nome-da-funcionalidade
```

Revise o diff antes do commit e confirme que não há credenciais, dumps do banco, uploads ou dependências geradas incluídos.

## 10. Checklist inicial

- [ ] Ambiente local configurado.
- [ ] WordPress e PHP em versões suportadas.
- [ ] Tema e plugins definidos.
- [ ] Configurações sensíveis fora do Git.
- [ ] Backup e restauração testados.
- [ ] Responsividade e acessibilidade verificadas.
- [ ] Formulários e integrações testados.
- [ ] Lint, testes e auditoria de dependências executados.
- [ ] Plano de publicação e rollback documentado.
