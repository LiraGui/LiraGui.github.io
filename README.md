# 📝 Meu Blog & Portfólio

Blog pessoal e portfólio desenvolvido com Jekyll e hospedado no GitHub Pages.

## 🚀 Sobre o Projeto

Este repositório contém o código-fonte do meu site pessoal, que funciona como:
- **Blog**: Para compartilhar artigos sobre tecnologia, programação e desenvolvimento
- **Portfólio**: Para showcasear projetos e trabalhos

## 🛠️ Tecnologias Utilizadas

- **Jekyll 4.3**: Gerador de sites estáticos
- **Minima Theme**: Tema minimalista e responsivo
- **GitHub Pages**: Hospedagem gratuita
- **Markdown**: Para escrita de conteúdo
- **Liquid**: Template engine

## 📋 Pré-requisitos

Para rodar o projeto localmente, você precisa ter instalado:

- Ruby (versão 2.7 ou superior)
- RubyGems
- GCC e Make

### Instalação no macOS

```bash
# Instalar Ruby via Homebrew
brew install ruby

# Adicionar ao PATH (adicione ao seu ~/.zshrc ou ~/.bash_profile)
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
```

### Instalação no Linux (Ubuntu/Debian)

```bash
sudo apt-get install ruby-full build-essential zlib1g-dev

# Configurar gems no diretório do usuário
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## 🔧 Instalação e Configuração

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/lira-blog.git
cd lira-blog
```

### 2. Instale as dependências

```bash
gem install bundler
bundle install
```

### 3. Configure o `_config.yml`

Edite o arquivo `_config.yml` e personalize com suas informações:

```yaml
title: Seu Nome
description: Sua descrição
author: Seu Nome
email: seu.email@example.com
url: "https://seu-usuario.github.io"
```

Atualize também os links das redes sociais.

### 4. Execute localmente

```bash
bundle exec jekyll serve
```

Acesse: `http://localhost:4000`

Para recarregar automaticamente ao fazer mudanças:

```bash
bundle exec jekyll serve --livereload
```

## 📁 Estrutura do Projeto

```
lira-blog/
├── _config.yml          # Configurações do site
├── _posts/              # Posts do blog (formato: YYYY-MM-DD-titulo.md)
├── _site/               # Site gerado (não versionado)
├── assets/              # Imagens, CSS, JS
│   └── css/
│       └── style.scss   # Estilos personalizados
├── index.md             # Página inicial
├── about.md             # Página sobre
├── portfolio.md         # Página de portfólio
├── blog.md              # Página do blog
├── Gemfile              # Dependências Ruby
└── README.md            # Este arquivo
```

## ✍️ Criando Posts

### 1. Crie um novo arquivo em `_posts/`

Nome do arquivo: `YYYY-MM-DD-titulo-do-post.md`

Exemplo: `2024-03-15-meu-primeiro-post.md`

### 2. Adicione o front matter

```markdown
---
layout: post
title: "Título do Seu Post"
date: 2024-03-15 10:00:00 -0300
categories: [categoria1, categoria2]
tags: [tag1, tag2, tag3]
author: Seu Nome
---

Conteúdo do seu post em Markdown...
```

### 3. Escreva o conteúdo em Markdown

Use Markdown para formatar o texto, adicionar imagens, links, código, etc.

## 🎨 Personalizações

### CSS Customizado

Edite `assets/css/style.scss` para personalizar a aparência do site.

### Adicionar Páginas

Crie novos arquivos `.md` na raiz com front matter:

```markdown
---
layout: page
title: Título da Página
permalink: /url-da-pagina/
---

Conteúdo...
```

## 🚀 Deploy no GitHub Pages

### 1. Crie um repositório no GitHub

Nome sugerido: `seu-usuario.github.io` (para site principal) ou qualquer outro nome.

### 2. Configure GitHub Pages

1. Vá em **Settings** > **Pages**
2. Em **Source**, selecione a branch `main` (ou `master`)
3. Clique em **Save**

### 3. Push para o repositório

```bash
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/seu-usuario/seu-repositorio.git
git push -u origin main
```

### 4. Acesse seu site

- Se o repositório for `seu-usuario.github.io`: `https://seu-usuario.github.io`
- Caso contrário: `https://seu-usuario.github.io/nome-do-repositorio`

## 📝 Dicas

### Testar antes de publicar

```bash
bundle exec jekyll build
bundle exec jekyll serve --drafts
```

### Criar rascunhos

Crie arquivos em `_drafts/` (sem data no nome) para posts em desenvolvimento.

### Ver posts futuros

```bash
bundle exec jekyll serve --future
```

### Limpar o cache

```bash
bundle exec jekyll clean
```

## 🐛 Troubleshooting

### Erro: `cannot load such file -- webrick`

```bash
bundle add webrick
```

### Problemas com permissões

```bash
sudo gem install bundler
```

### Site não atualiza no GitHub Pages

- Aguarde alguns minutos (pode levar até 10 minutos)
- Verifique a aba **Actions** do repositório para ver o status do build
- Certifique-se de que o `_config.yml` está correto

## 📚 Recursos

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Minima Theme](https://github.com/jekyll/minima)
- [Markdown Guide](https://www.markdownguide.org/)
- [Liquid Template Language](https://shopify.github.io/liquid/)

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 👤 Autor

**Seu Nome**

- GitHub: [@seu-usuario](https://github.com/seu-usuario)
- LinkedIn: [seu-perfil](https://linkedin.com/in/seu-perfil)
- Email: seu.email@example.com

## 🤝 Contribuições

Contribuições, issues e pull requests são bem-vindos!

---

⭐ Se este projeto foi útil, considere dar uma estrela!

**Feito com ❤️ usando Jekyll e GitHub Pages**
