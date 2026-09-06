# Orçamento Globo

Ferramenta de orçamento (venda e manutenção de extintores) da Extintores Globo — um único arquivo HTML estático, sem backend, sem build.

## Deploy no Vercel

**Opção 1 — pelo site (mais fácil, sem instalar nada):**

1. Entre em https://vercel.com e faça login (dá pra usar conta Google/GitHub).
2. Clique em **Add New → Project**.
3. Quando pedir um repositório, escolha **"Deploy without Git"** / arraste a pasta, ou primeiro suba esta pasta para um repositório novo no GitHub e importe esse repositório.
4. Ele vai detectar que é um site estático (sem framework) — não precisa configurar build command nem output directory, pode deixar em branco/"Other".
5. Clique em **Deploy**. Em menos de 1 minuto você recebe uma URL tipo `seu-projeto.vercel.app`.

**Opção 2 — pelo terminal (Vercel CLI):**

```bash
npm i -g vercel
cd orcamento-globo
vercel        # segue as perguntas (primeira vez faz login)
vercel --prod # publica em produção
```

## Domínio próprio

Depois do deploy, em **Project → Settings → Domains** dá pra apontar um domínio seu (ex: orcamento.extintoresglobo.com.br) — o Vercel mostra o registro DNS (CNAME/A) para configurar no seu provedor.

## Sobre os dados

**Clientes** ficam no **Firestore** (Firebase) quando ele está configurado — aí você e seu sócio veem a mesma lista, de qualquer aparelho. Sem Firebase configurado, os clientes ficam só no navegador daquele computador.

**Tabela de preços e dados da empresa** ficam sempre no `localStorage` do navegador (são configurações de cada máquina).

### Conectar o Firebase

1. No [console do Firebase](https://console.firebase.google.com), abra o projeto → ⚙ **Configurações do projeto** → aba **Geral**.
2. Em **Seus apps**, crie (ou abra) um app da **Web** (ícone `</>`).
3. Em **Configuração do SDK**, escolha **Config** e copie o objeto `firebaseConfig` inteiro.
4. No orçamento, vá em **Empresa → Banco de dados (Firebase)**, cole o objeto e clique em **Conectar**.
5. Se já tinha clientes salvos naquele navegador, clique em **Enviar clientes locais para a nuvem**.

A configuração fica salva no navegador. Para nascer já conectado em qualquer aparelho, dá para colar o mesmo objeto direto no `index.html`, na constante `FIREBASE_CONFIG_PADRAO`.

> O `firebaseConfig` da web é **público por natureza** (vai no HTML de qualquer site Firebase) — quem protege os dados são as **regras do Firestore**, abaixo.

### Regras do Firestore

Em **Firestore Database → Regras**. Enquanto for só vocês dois, o mínimo aceitável é limitar por data de expiração:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /clientes/{doc} {
      allow read, write: if request.time < timestamp.date(2027, 1, 1);
    }
  }
}
```

Isso deixa **qualquer pessoa com o link** ler e gravar até essa data. Para valer de verdade, o certo é ativar **Authentication** (login por e-mail/senha para você e seu sócio) e trocar a regra por `if request.auth != null` — dá para fazer isso depois, sem mexer no resto.

### Aviso de renovação

Cada cliente guarda a **data da última manutenção** e a **periodicidade** (12 meses por padrão). Ao abrir o app, se algum cliente estiver vencido ou vencendo nos próximos 30 dias (ajustável em **Empresa**), aparece um pop-up com a lista. O botão **"Renovei"** grava a data de hoje — ou seja, um cliente de setembro volta a aparecer em setembro do ano seguinte.

O pop-up aparece no máximo 1x por dia; "Lembrar daqui a 7 dias" adia.

## Arquivos

- `index.html` — a ferramenta inteira (HTML + CSS + JS em um arquivo só)
- `logo-globo.jpg` — logo padrão da proposta (trocável pela aba Empresa)
- `vercel.json` — configuração mínima do Vercel
