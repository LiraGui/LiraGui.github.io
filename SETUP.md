# 🚀 Guia Rápido de Setup - Blog & Portfólio

Este é um guia passo a passo para configurar e fazer deploy do seu blog e portfólio no GitHub Pages.

## ✅ Checklist de Configuração

### 1. Personalize suas informações

#### `_config.yml`
- [ ] Altere `title` para o nome do seu blog
- [ ] Altere `author` para o seu nome
- [ ] Altere `email` para o seu email
- [ ] Altere `url` para `https://seu-usuario.github.io`
- [ ] Atualize os links das redes sociais (GitHub, LinkedIn, Twitter)

#### `about.md`
- [ ] Complete sua formação acadêmica
- [ ] Adicione sua experiência profissional
- [ ] Liste suas habilidades técnicas
- [ ] Atualize seus objetivos e interesses
- [ ] Adicione seus links de contato

#### `portfolio.md`
- [ ] Adicione seus projetos principais
- [ ] Inclua screenshots dos projetos (em `/assets/images/`)
- [ ] Atualize os links para demos e código fonte
- [ ] Liste contribuições open source

#### `index.md`
- [ ] Personalize a mensagem de boas-vindas
- [ ] Atualize suas áreas de interesse

### 2. Crie conteúdo

#### Posts
- [ ] Revise os posts de exemplo em `_posts/`
- [ ] Crie seus próprios posts seguindo o formato `YYYY-MM-DD-titulo.md`
- [ ] Adicione imagens em `/assets/images/`

#### Imagens
Crie a pasta e adicione imagens para o portfólio:
```bash
mkdir -p assets/images
# Adicione suas imagens aqui
```

### 3. Configure o repositório no GitHub

#### Opção A: Site principal (recomendado)
1. Crie um repositório chamado `seu-usuario.github.io`
2. Seu site ficará em: `https://seu-usuario.github.io`

#### Opção B: Projeto específico
1. Crie um repositório com qualquer nome (ex: `blog`)
2. No `_config.yml`, altere `baseurl: "/blog"`
3. Seu site ficará em: `https://seu-usuario.github.io/blog`

### 4. Ative GitHub Pages

1. Vá em **Settings** > **Pages**
2. Em **Source**, selecione:
   - **GitHub Actions** (recomendado) - usa o workflow já incluído
   - OU **Deploy from a branch** e selecione `main` branch

### 5. Push inicial

```bash
# Adicione todos os arquivos
git add .

# Faça o commit inicial
git commit -m "Initial commit: Blog e Portfólio configurado"

# Adicione o remote (substitua seu-usuario)
git remote add origin https://github.com/seu-usuario/seu-repositorio.git

# Push para o GitHub
git branch -M main
git push -u origin main
```

### 6. Teste localmente (opcional mas recomendado)

```bash
# Instale as dependências
bundle install

# Execute o servidor local
bundle exec jekyll serve

# Acesse: http://localhost:4000
```

## 🎨 Personalizações Adicionais

### Tema Dark/Light
Altere em `_config.yml`:
```yaml
minima:
  skin: dark  # opções: auto, classic, dark, solarized, solarized-dark
```

### Google Analytics
Descomente e adicione seu ID em `_config.yml`:
```yaml
google_analytics: UA-XXXXXXXXX-X
```

### Comentários com Disqus
Descomente e adicione seu shortname em `_config.yml`:
```yaml
disqus:
  shortname: seu-shortname-disqus
```

### Cores e CSS
Edite `assets/css/style.scss` para personalizar cores, fontes e estilos.

## 📝 Workflow de Publicação

### Criar novo post
```bash
# 1. Crie o arquivo
touch _posts/2024-03-15-titulo-do-post.md

# 2. Adicione o front matter e conteúdo
# (veja exemplo nos posts existentes)

# 3. Teste localmente
bundle exec jekyll serve --drafts

# 4. Commit e push
git add _posts/2024-03-15-titulo-do-post.md
git commit -m "Adiciona post: Título do Post"
git push
```

### Atualizar portfólio
```bash
# 1. Edite portfolio.md
# 2. Adicione imagens em assets/images/
# 3. Commit e push
git add portfolio.md assets/images/
git commit -m "Atualiza portfólio com novo projeto"
git push
```

## 🔍 Verificação Final

Antes de compartilhar seu site, verifique:

- [ ] Site carrega corretamente (sem erros 404)
- [ ] Todos os links funcionam
- [ ] Imagens aparecem corretamente
- [ ] Links de redes sociais estão corretos
- [ ] Posts estão formatados corretamente
- [ ] Site é responsivo (teste em mobile)
- [ ] Meta tags e SEO estão configurados
- [ ] Favicon está adicionado (opcional)

## 🐛 Problemas Comuns

### Site não atualiza
- Aguarde 2-10 minutos após o push
- Verifique **Actions** tab no GitHub
- Limpe o cache do navegador

### Erro 404
- Verifique se o `baseurl` está correto no `_config.yml`
- Para site principal (username.github.io), deixe `baseurl: ""`

### Build falha
- Verifique o log em **Actions**
- Teste localmente com `bundle exec jekyll build`
- Verifique erros de sintaxe no YAML (front matter)

### CSS não carrega
- Verifique o front matter (`---`) em `style.scss`
- Limpe o cache: `bundle exec jekyll clean`

## 📚 Próximos Passos

Após setup inicial:

1. **SEO**: Adicione meta descriptions nos posts
2. **Analytics**: Configure Google Analytics
3. **RSS**: O feed RSS já está em `/feed.xml`
4. **Sitemap**: O sitemap já está em `/sitemap.xml`
5. **Custom Domain**: Configure um domínio personalizado (opcional)
6. **Newsletter**: Integre com Mailchimp ou similar
7. **Comentários**: Configure Disqus ou outro sistema

## 🎉 Pronto!

Seu blog e portfólio estão configurados! Agora é só criar conteúdo e compartilhar.

**Dúvidas?** Consulte o [README.md](README.md) completo ou a [documentação do Jekyll](https://jekyllrb.com/docs/).

---

**Boa sorte com seu blog! 🚀**
