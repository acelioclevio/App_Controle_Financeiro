# Guia de Implantação - Controle Financeiro PWA

## 1. Preparação do Projeto no GitHub

1. Crie uma conta no GitHub (se ainda não tiver)
2. Crie um novo repositório:
   - Acesse github.com e clique em "New repository"
   - Nome: finance-app (ou outro de sua preferência)
   - Descrição: Aplicativo de Controle Financeiro
   - Selecione "Public"
   - Marque "Add a README file"
   - Clique em "Create repository"

3. Configure o projeto localmente:
```bash
# Clone o repositório
git clone https://github.com/seu-usuario/finance-app.git
cd finance-app

# Crie o projeto React com Vite
npm create vite@latest . -- --template react

# Instale as dependências
npm install
npm install @/components/ui

# Adicione os arquivos do projeto
# (Copie todos os arquivos do aplicativo para a pasta src/)

# Commit e push das alterações
git add .
git commit -m "Initial commit"
git push origin main
```

## 2. Configuração no Netlify

1. Crie uma conta no Netlify (netlify.com)
2. Conecte com GitHub:
   - Clique em "New site from Git"
   - Escolha "GitHub"
   - Autorize o Netlify no GitHub
   - Selecione o repositório `finance-app`

3. Configure a implantação:
   - Build command: `npm run build`
   - Publish directory: `dist`
   - Clique em "Deploy site"

4. Configure o domínio personalizado (opcional):
   - Vá para "Site settings" > "Domain management"
   - Clique em "Add custom domain"
   - Siga as instruções para configurar seu domínio

## 3. Configuração do PWA

1. Adicione as configurações do PWA no `vite.config.js`:
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    react(),
    VitePWA({
      registerType: 'autoUpdate',
      manifest: {
        name: 'Controle Financeiro',
        short_name: 'Finanças',
        description: 'Aplicativo de controle financeiro pessoal',
        theme_color: '#ffffff',
        icons: [
          {
            src: '/icon-192x192.png',
            sizes: '192x192',
            type: 'image/png'
          },
          {
            src: '/icon-512x512.png',
            sizes: '512x512',
            type: 'image/png'
          }
        ]
      }
    })
  ]
})
```

2. Instale as dependências do PWA:
```bash
npm install vite-plugin-pwa -D
```

3. Adicione os ícones:
   - Crie/copie os ícones para a pasta `public/`
   - Certifique-se de ter `icon-192x192.png` e `icon-512x512.png`

4. Atualize o repositório:
```bash
git add .
git commit -m "Add PWA configuration"
git push origin main
```

## 4. Configurações Finais no Netlify

1. Configure redirecionamentos para SPA:
   - Crie um arquivo `_redirects` na pasta `public/`:
```
/* /index.html 200
```

2. Configure headers para PWA:
   - Crie um arquivo `netlify.toml` na raiz do projeto:
```toml
[[headers]]
  for = "/manifest.webmanifest"
  [headers.values]
    Content-Type = "application/manifest+json"

[[headers]]
  for = "/assets/*"
  [headers.values]
    cache-control = '''
    max-age=31536000,
    immutable
    '''
```

3. Faça deploy das alterações:
```bash
git add .
git commit -m "Add Netlify configuration"
git push origin main
```

## 5. Verificação

1. Aguarde o deploy automático no Netlify
2. Teste o site publicado:
   - Acesse a URL fornecida pelo Netlify
   - Verifique se o aplicativo carrega corretamente
   - Teste a instalação do PWA em diferentes dispositivos
   - Verifique se os dados estão sendo salvos corretamente
   - Teste o funcionamento offline

## 6. Manutenção

Para atualizar o aplicativo:
1. Faça as alterações necessárias localmente
2. Teste as mudanças
3. Commit e push para o GitHub
4. O Netlify fará o deploy automaticamente

## Solução de Problemas Comuns

1. Se o PWA não estiver instalável:
   - Verifique se os ícones estão presentes e acessíveis
   - Confirme se o manifest está sendo servido corretamente
   - Use o Chrome DevTools (aba Application) para diagnosticar

2. Se o site não carregar:
   - Verifique os logs de build no Netlify
   - Confirme se todas as dependências estão instaladas
   - Verifique se o arquivo `_redirects` está presente

3. Se as atualizações não aparecerem:
   - Limpe o cache do navegador
   - Verifique se o service worker está registrado
   - Force uma atualização do PWA
