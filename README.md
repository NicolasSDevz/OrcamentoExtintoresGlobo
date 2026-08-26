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

Tudo (clientes, orçamentos, tabela de preços, dados da empresa) fica salvo no **localStorage do navegador** de quem estiver usando — não existe banco de dados nem login. Ou seja:

- Os dados ficam só naquele navegador/computador específico, não sincronizam entre dispositivos.
- Limpar o cache do navegador ou trocar de computador apaga o histórico.
- Se vocês dois (você e seu sócio) forem usar em máquinas diferentes, cada um vai ter sua própria lista de orçamentos — não é compartilhado automaticamente.

Se no futuro quiser que os orçamentos fiquem centralizados (os dois veem os mesmos dados, de qualquer aparelho), é preciso adicionar um banco de dados de verdade — dá para evoluir esse mesmo projeto para isso quando quiser.

## Arquivos

- `index.html` — a ferramenta inteira (HTML + CSS + JS em um arquivo só)
- `vercel.json` — configuração mínima do Vercel
