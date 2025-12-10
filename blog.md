---
layout: page
title: Blog
permalink: /blog/
---

# Blog 📝

Bem-vindo ao meu blog! Aqui compartilho artigos sobre desenvolvimento de software, tecnologia, tutoriais e minhas experiências como desenvolvedor.

## 📚 Categorias

- **Tutoriais**: Guias passo a passo e how-tos
- **Desenvolvimento Web**: Frontend, Backend, Full-stack
- **DevOps**: CI/CD, Docker, Kubernetes, Cloud
- **Carreira**: Dicas, experiências e aprendizados
- **Tecnologia**: Tendências e novidades do mundo tech

---

## 🔍 Todos os Posts

<div class="posts-list">
  {% for post in site.posts %}
    <article class="post-preview">
      <h3>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h3>
      
      <p class="post-meta">
        <time datetime="{{ post.date | date_to_xmlschema }}">
          {{ post.date | date: "%d de %B de %Y" }}
        </time>
        {% if post.author %}
          • <span>{{ post.author }}</span>
        {% endif %}
      </p>
      
      {% if post.categories %}
        <div class="post-categories">
          {% for category in post.categories %}
            <span class="category-badge">{{ category }}</span>
          {% endfor %}
        </div>
      {% endif %}
      
      <div class="post-excerpt">
        {{ post.excerpt | strip_html | truncatewords: 50 }}
      </div>
      
      <a href="{{ post.url | relative_url }}" class="read-more">
        Ler mais →
      </a>
    </article>
    <hr>
  {% endfor %}
</div>

{% if site.posts.size == 0 %}
  <p><em>Nenhum post publicado ainda. Volte em breve!</em></p>
{% endif %}

---

## 📬 Newsletter

Quer receber novos posts por email? [Inscreva-se na newsletter](#)!

## 🔖 Tags Populares

{% if site.tags %}
  <div class="tags-cloud">
    {% for tag in site.tags %}
      <a href="#{{ tag[0] }}" class="tag">{{ tag[0] }} ({{ tag[1].size }})</a>
    {% endfor %}
  </div>
{% endif %}
