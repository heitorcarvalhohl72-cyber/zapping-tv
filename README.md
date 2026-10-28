# ZappingTV

Aplicação web responsiva de catálogo de canais de televisão, com player Clappr preparado para transmissões HLS autorizadas. O projeto inclui catálogo inicial, busca, categorias, favoritos, histórico local, tema claro/escuro e painel administrativo protegido por senha.

## Requisitos

- Node.js 18 ou superior
- Conexão com a internet para carregar o Clappr pela CDN (ou hospede os arquivos localmente)
- URLs HTTPS de transmissões HLS (`.m3u8`) que você tenha autorização para reproduzir e distribuir

## Instalação

1. Extraia o ZIP e abra a pasta `zappingtv`.
2. Instale as dependências:

   ```bash
   npm install
   ```

3. Copie `.env.example` para `.env` e altere `ADMIN_PASSWORD` para uma senha forte. Nunca publique o `.env`.
4. Inicie:

   ```bash
   npm start
   ```

5. Abra `http://localhost:3000` no navegador.

## Como configurar uma transmissão autorizada

1. Abra o ZappingTV e selecione o ícone de engrenagem.
2. Entre usando a senha configurada em `ADMIN_PASSWORD`.
3. Localize o canal e informe uma URL HTTPS HLS autorizada, por exemplo `https://seu-dominio.example/live/playlist.m3u8` (endereço ilustrativo, não é um stream real).
4. Opcionalmente, informe uma URL HTTPS para o logotipo e clique em **Salvar alterações**.
5. Abra o canal para testar a reprodução.

O servidor valida o formato básico da URL; isso não garante que o stream esteja online, autorizado, compatível com CORS ou com o navegador. Não use links privados, extraídos de serviços de assinatura, nem contorne DRM ou autenticação.

## Dados do catálogo

O catálogo está em `data/channels.json`. Cada registro possui:

- `id`: identificador único
- `name`: nome do canal
- `logo`: URL opcional de logotipo
- `categories`: uma ou mais categorias
- `streamUrl`: URL HLS autorizada ou string vazia
- `status`: `configured` ou `unconfigured`
- `provider`: operadora/provedor, opcional
- `epg`: lista de programação autorizada (vazia no catálogo inicial)
- `demo`: indica que é um registro inicial demonstrativo

As categorias podem ser editadas no JSON. A interface usa as categorias definidas no array `CATEGORIES` em `public/app.js`.

## Segurança e produção

Este projeto é uma base funcional para desenvolvimento, não uma implantação de produção pronta para internet aberta. Antes de publicar:
- Use HTTPS e um proxy reverso.
- Troque o mecanismo de senha simples por autenticação robusta, sessões seguras, rate limiting e proteção CSRF.
- Restrinja o painel administrativo a usuários autorizados.
- Não armazene senhas, tokens privados ou credenciais no frontend.
- Considere um banco de dados, logs, backups e validação de permissões.
- Configure CORS nos servidores de mídia quando necessário. O player não consegue superar CORS, DRM ou restrições do provedor.
- Confirme licenças e autorizações para logotipos, marcas, EPG e transmissões.

## Smart TVs e Chromecast

A interface funciona em navegadores modernos e adapta-se a telas pequenas e grandes. Compatibilidade real depende do navegador da TV, suporte a JavaScript, codecs, HLS e desempenho do dispositivo. Para navegação por controle remoto, a interface inclui foco por teclado nos cartões; teste com cada modelo. Chromecast exige integração de Cast SDK e receiver compatível, não incluída nesta versão.

## Estrutura

```text
zappingtv/
├── data/channels.json
├── public/
│   ├── app.js
│   ├── index.html
│   └── styles.css
├── .env.example
├── package.json
├── README.md
└── server.js
```

## Limitações atuais

- O catálogo é demonstrativo; não inclui transmissões reais.
- Nenhuma afiliação oficial com Claro, Vivo, SKY ou Oi é presumida.
- EPG, autenticação de operadoras, DRM, casting e apps nativos ainda exigem integrações autorizadas específicas.
- O painel protege alterações com senha via API, mas deve ser endurecido antes de exposição pública.
- 
