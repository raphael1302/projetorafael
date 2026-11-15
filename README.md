# Compras Binho

Sistema de **compras e almoxarifado** da Binho Transportes (protótipo funcional), com publicação automática
do site e geração dos apps de **Android** e **iPhone** a partir do mesmo arquivo.

A página inicial é o **`index.html` da raiz**: é ele que abre no GitHub Pages, no Cloudflare Pages e dentro dos apps.

## O que tem aqui

```
compras-binho/
├── index.html                ← o sistema inteiro (página inicial)
├── teste-camera.html         ← teste rápido da câmera ao vivo
├── _headers                  ← cabeçalhos para o Cloudflare Pages (libera a câmera, protege o site)
├── .nojekyll                 ← o GitHub Pages entrega os arquivos como estão
├── apps/
│   ├── android/              ← app Android (WebView com câmera liberada)
│   └── ios/                  ← app iPhone (WebView, TestFlight)
├── docs/                     ← guias: publicar o site, Android, iPhone
├── scripts/atualizar-sistema.sh
└── .github/workflows/
    ├── github-pages.yml      ← publica o index.html no GitHub Pages
    ├── site-cloudflare.yml   ← alternativa: Cloudflare Pages com login
    ├── android-apk.yml       ← gera o APK
    └── ios-testflight.yml    ← app iPhone / TestFlight
```

Os workflows de site publicam **só** `index.html` e `teste-camera.html`: as pastas dos apps e os guias nunca vão para o ar.
A chave de assinatura do Android **não** está no repositório: ela vem numa pasta separada e entra como segredo do GitHub.

## Passo 1 — Colocar no GitHub

1. Instale o **GitHub Desktop** (<https://desktop.github.com>).
2. Descompacte o pacote. Guarde a pasta **`chave-android-NAO-ENVIAR-AO-GITHUB`** em local seguro e **não** a coloque no repositório.
3. No GitHub Desktop: **File > Add local repository**, escolha a pasta `compras-binho`, confirme **create a repository** e depois **Publish repository**.

Pelo site também funciona (**uploading an existing file**), mas confira se as pastas ocultas `.github` e o arquivo `.nojekyll` foram junto.

## Passo 2 — Página inicial no GitHub Pages

1. No repositório: **Settings > Pages > Build and deployment > Source: GitHub Actions**.
2. Aba **Actions > Site - publicar no GitHub Pages > Run workflow** (depois roda sozinho a cada mudança no `index.html`).
3. O endereço aparece na execução e em **Settings > Pages**, no formato `https://SEU-USUARIO.github.io/compras-binho/`.
4. No celular, abra esse endereço direto no **Chrome** ou no **Safari**: a câmera ao vivo funciona, porque o GitHub Pages é https e não exibe o sistema dentro de outra página.

> **Antes de publicar, leia:** no plano gratuito, o GitHub Pages só funciona com **repositório público**, e o site
> fica aberto para qualquer pessoa com o link. O `index.html` traz os dados exportados do SSW
> (CPFs, contas bancárias e chaves PIX de 16.248 fornecedores). Publicando assim, esses dados ficam públicos
> no repositório e no site. Para manter os dados protegidos, use o repositório **privado** com o
> **Cloudflare Pages com login** ([docs/PUBLICAR-SITE.md](docs/PUBLICAR-SITE.md)), que também é gratuito.

## Passo 3 — Apps

| App | Guia | Precisa de |
|---|---|---|
| Android (APK, sem loja) | [docs/GUIA-ANDROID.md](docs/GUIA-ANDROID.md) | Segredos da chave (pasta separada) |
| iPhone (TestFlight) | [docs/GUIA-IPHONE.md](docs/GUIA-IPHONE.md) | Conta Apple Developer (US$ 99/ano) |

## Atualizar o sistema

1. Substitua o `index.html` da raiz pela versão nova (ou `./scripts/atualizar-sistema.sh caminho/sistema-compras.html`).
2. GitHub Desktop: **Commit to main** e **Push origin**.
3. Automático: o site é republicado e um APK novo é gerado. Para o iPhone, rode o workflow do TestFlight.

## Limites do protótipo

- Os dados ficam só na memória do navegador/app: recarregar volta aos dados de exemplo. Para uso definitivo falta
  um servidor com banco de dados e login próprio.
- Integrações com SSW e Mitra são simuladas; a lista de fornecedores é uma exportação do SSW embutida no arquivo.
