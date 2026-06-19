---
name: site-institucional
description: >
  Cria sites institucionais completos, modernos e responsivos usando HTML semântico, Tailwind CSS e animações AOS.
  Use SEMPRE que o usuário pedir para criar, desenvolver ou construir um site institucional, site para empresa, landing page institucional, site para negócio local, site para clínica, escritório, oficina, loja, restaurante, ou qualquer outro tipo de site de apresentação empresarial.
  Também acione quando o usuário disser "faz um site para meu cliente", "preciso de um site", "cria um site", "desenvolve um site", mesmo sem mencionar explicitamente "institucional".
  Esta skill garante padrão profissional com light/dark mode, menu hamburger no mobile, animações AOS, scroll suave, popup LGPD, barra de scroll customizada, botões animados e commit automático das alterações.
---

# Skill — Desenvolvimento de Sites Institucionais

## Visão Geral

Esta skill define os padrões obrigatórios para criar sites institucionais para clientes. Todo site gerado deve seguir **rigorosamente** todas as diretrizes abaixo, sem exceção.

---

## Stack Obrigatória

- **HTML5 semântico** — tags corretas: `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`
- **Tailwind CSS** — via CDN ou configurado via npm/vite dependendo do projeto
- **AOS (Animate On Scroll)** — para todas as animações de entrada de elementos
- **Google Fonts** — fonte escolhida conforme o nicho do cliente
- **Nenhuma dependência desnecessária** — manter o projeto enxuto

---

## Processo de Criação

### 1. Briefing (perguntar antes de começar)

Antes de criar qualquer código, colete:
- Nome da empresa / marca
- Nicho / segmento de atuação
- Cores da identidade visual (ou sugerir baseado no nicho)
- Seções necessárias (padrão: Hero, Sobre, Serviços, Depoimentos, Contato)
- Informações de contato (telefone, WhatsApp, e-mail, endereço)
- Links de redes sociais
- Preferência de estilo (minimalista, arrojado, elegante, etc.)

Se o usuário já forneceu essas informações na conversa, use-as diretamente sem perguntar novamente.

### 2. Escolha da Fonte

Sempre escolha fontes do Google Fonts adequadas ao nicho:

| Nicho | Fontes Sugeridas |
|---|---|
| Jurídico / Advocacia | Playfair Display + Source Sans 3 |
| Saúde / Clínica / Médico | Lato + Nunito |
| Tecnologia / Software | Inter + JetBrains Mono |
| Beleza / Estética | Cormorant Garamond + Jost |
| Gastronomia / Restaurante | Libre Baskerville + Raleway |
| Automotivo / Mecânica / Náutico | Bebas Neue + Roboto |
| Construção / Engenharia | Barlow + Open Sans |
| Moda / Lifestyle | Italiana + Poppins |
| Educação / Cursos | Merriweather + Mulish |
| Financeiro / Contabilidade | Libre Franklin + DM Sans |
| Genérico / Corporativo | Plus Jakarta Sans + Inter |

### 3. Estrutura de Arquivos

```
projeto-cliente/
├── index.html
├── contato.html (se necessário)
├── assets/
│   ├── css/
│   │   └── custom.css   (apenas overrides do Tailwind e scroll bar)
│   ├── js/
│   │   └── main.js      (menu hamburger, AOS init, LGPD popup)
│   └── img/
│       └── (imagens do cliente)
└── README.md
```

---

## Padrões Obrigatórios de Código

### Light / Dark Mode

```html
<!-- Classe dark no <html> controlada via JS -->
<html lang="pt-BR" class="scroll-smooth">
```

```javascript
// Sempre incluir toggle de tema
const theme = localStorage.getItem('theme') || 'light';
document.documentElement.classList.toggle('dark', theme === 'dark');

function toggleTheme() {
  const isDark = document.documentElement.classList.toggle('dark');
  localStorage.setItem('theme', isDark ? 'dark' : 'light');
}
```

```html
<!-- Botão de toggle no header -->
<button onclick="toggleTheme()" class="...">
  <svg id="icon-sun" ...><!-- ícone sol --></svg>
  <svg id="icon-moon" ...><!-- ícone lua --></svg>
</button>
```

Configuração Tailwind para dark mode:
```html
<script>
  tailwind.config = {
    darkMode: 'class',
    theme: {
      extend: {
        // cores customizadas do projeto
      }
    }
  }
</script>
```

---

### Menu Hamburger (Mobile)

```html
<nav>
  <!-- Desktop: links normais -->
  <div class="hidden md:flex gap-8">
    <a href="#sobre">Sobre</a>
    <!-- ... -->
  </div>

  <!-- Mobile: botão hamburger -->
  <button id="menu-toggle" class="md:hidden" aria-label="Abrir menu">
    <span class="block w-6 h-0.5 bg-current transition-all"></span>
    <span class="block w-6 h-0.5 bg-current transition-all mt-1.5"></span>
    <span class="block w-6 h-0.5 bg-current transition-all mt-1.5"></span>
  </button>
</nav>

<!-- Drawer lateral mobile -->
<div id="mobile-menu" class="fixed inset-y-0 right-0 w-72 z-50 transform translate-x-full transition-transform duration-300 ease-in-out bg-white dark:bg-gray-900 shadow-2xl">
  <div class="flex flex-col p-8 gap-6 mt-16">
    <a href="#sobre" class="mobile-link text-xl font-medium">Sobre</a>
    <!-- ... -->
  </div>
</div>

<!-- Overlay -->
<div id="overlay" class="fixed inset-0 bg-black/50 z-40 hidden opacity-0 transition-opacity duration-300"></div>
```

```javascript
// Menu hamburger - drawer lateral
const menuToggle = document.getElementById('menu-toggle');
const mobileMenu = document.getElementById('mobile-menu');
const overlay = document.getElementById('overlay');

function openMenu() {
  mobileMenu.classList.remove('translate-x-full');
  overlay.classList.remove('hidden');
  setTimeout(() => overlay.classList.add('opacity-100'), 10);
  document.body.classList.add('overflow-hidden');
}

function closeMenu() {
  mobileMenu.classList.add('translate-x-full');
  overlay.classList.remove('opacity-100');
  setTimeout(() => overlay.classList.add('hidden'), 300);
  document.body.classList.remove('overflow-hidden');
}

menuToggle.addEventListener('click', openMenu);
overlay.addEventListener('click', closeMenu);

// Fechar ao clicar em link
document.querySelectorAll('.mobile-link').forEach(link => {
  link.addEventListener('click', closeMenu);
});
```

> ⚠️ **NUNCA usar dois scroll bars.** O drawer usa `position: fixed` e o body recebe `overflow-hidden` ao abrir. Revisar sempre antes de entregar.

---

### AOS — Animate On Scroll

```html
<!-- No <head> -->
<link rel="stylesheet" href="https://unpkg.com/aos@2.3.1/dist/aos.css" />

<!-- Antes do </body> -->
<script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
<script>
  AOS.init({
    duration: 800,
    easing: 'ease-out-cubic',
    once: true,
    offset: 80
  });
</script>
```

Atributos AOS nos elementos:
```html
<section data-aos="fade-up">...</section>
<div data-aos="fade-right" data-aos-delay="100">...</div>
<div data-aos="fade-left" data-aos-delay="200">...</div>
<div data-aos="zoom-in" data-aos-delay="300">...</div>
```

---

### Scroll Smooth + Barra de Scroll Customizada

```html
<html class="scroll-smooth">
```

```css
/* assets/css/custom.css */

/* Barra de scroll na identidade do site */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: var(--color-primary); /* cor primária do projeto */
  border-radius: 100px;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--color-primary-dark);
}
```

> Adaptar `--color-primary` para a cor principal do cliente. Exemplo: azul para tecnologia, dourado para advocacia, etc.

---

### Botões Animados

Todos os botões do site devem ter animação. Usar ao menos **uma** das seguintes técnicas:

```html
<!-- Opção 1: Pulse suave (CTA principal) -->
<button class="animate-pulse-soft bg-primary text-white px-8 py-4 rounded-full font-semibold
               hover:scale-105 hover:shadow-lg hover:shadow-primary/40
               transition-all duration-300 cursor-pointer">
  Agende agora
</button>

<!-- Opção 2: Seta animada -->
<button class="group flex items-center gap-2 ...">
  Saiba mais
  <span class="transition-transform duration-300 group-hover:translate-x-2">→</span>
</button>

<!-- Opção 3: Brilho deslizante (shimmer) -->
<button class="relative overflow-hidden ...">
  <span class="relative z-10">Entre em contato</span>
  <span class="absolute inset-0 -translate-x-full hover:translate-x-0
               bg-white/20 transition-transform duration-500 skew-x-12"></span>
</button>
```

Adicionar no Tailwind config para pulse customizado:
```javascript
tailwind.config = {
  theme: {
    extend: {
      animation: {
        'pulse-soft': 'pulse-soft 2s ease-in-out infinite',
      },
      keyframes: {
        'pulse-soft': {
          '0%, 100%': { transform: 'scale(1)', boxShadow: '0 0 0 0 rgba(var(--color-primary-rgb), 0.4)' },
          '50%': { transform: 'scale(1.03)', boxShadow: '0 0 0 8px rgba(var(--color-primary-rgb), 0)' },
        }
      }
    }
  }
}
```

---

### Hover Effects

Aplicar em cards, links de navegação e imagens:

```html
<!-- Card com hover -->
<div class="group bg-white dark:bg-gray-800 rounded-2xl p-6 shadow-sm
            hover:shadow-xl hover:-translate-y-1 transition-all duration-300 cursor-pointer">
  <div class="w-12 h-12 bg-primary/10 rounded-xl flex items-center justify-center
              group-hover:bg-primary group-hover:scale-110 transition-all duration-300">
    <!-- ícone -->
  </div>
  <h3 class="mt-4 font-semibold group-hover:text-primary transition-colors duration-300">Título</h3>
</div>

<!-- Link de nav com underline animado -->
<a href="#" class="relative after:absolute after:bottom-0 after:left-0
                   after:w-0 after:h-0.5 after:bg-primary
                   after:transition-all after:duration-300 hover:after:w-full">
  Menu Item
</a>

<!-- Imagem com zoom no hover -->
<div class="overflow-hidden rounded-2xl">
  <img class="w-full h-64 object-cover scale-100 hover:scale-110 transition-transform duration-500" src="..." />
</div>
```

---

### Popup LGPD

Posicionado no **canto inferior direito**. Pequeno e discreto.

```html
<!-- Popup LGPD - canto inferior direito -->
<div id="lgpd-popup"
     class="fixed bottom-4 right-4 z-50 max-w-xs bg-white dark:bg-gray-800
            rounded-2xl shadow-2xl p-4 border border-gray-100 dark:border-gray-700
            transform translate-y-4 opacity-0 transition-all duration-500">
  <p class="text-xs text-gray-600 dark:text-gray-400 leading-relaxed">
    🍪 Usamos cookies para melhorar sua experiência.
    <a href="/politica-de-privacidade" class="text-primary underline">Saiba mais</a>
  </p>
  <div class="flex gap-2 mt-3">
    <button onclick="acceptLGPD()"
            class="flex-1 bg-primary text-white text-xs py-2 px-3 rounded-lg
                   hover:opacity-90 transition-opacity font-medium">
      Aceitar
    </button>
    <button onclick="closeLGPD()"
            class="flex-1 border border-gray-200 dark:border-gray-600 text-xs py-2 px-3
                   rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">
      Fechar
    </button>
  </div>
</div>
```

```javascript
// LGPD Popup
function showLGPD() {
  if (localStorage.getItem('lgpd-accepted')) return;
  const popup = document.getElementById('lgpd-popup');
  setTimeout(() => {
    popup.classList.remove('translate-y-4', 'opacity-0');
    popup.classList.add('translate-y-0', 'opacity-100');
  }, 2000);
}

function acceptLGPD() {
  localStorage.setItem('lgpd-accepted', 'true');
  closeLGPD();
}

function closeLGPD() {
  const popup = document.getElementById('lgpd-popup');
  popup.classList.add('translate-y-4', 'opacity-0');
}

showLGPD();
```

---

### Responsividade

Breakpoints Tailwind obrigatórios em todo componente:

| Prefixo | Tamanho |
|---|---|
| `sm:` | ≥ 640px |
| `md:` | ≥ 768px |
| `lg:` | ≥ 1024px |
| `xl:` | ≥ 1280px |

Sempre testar mentalmente nos tamanhos: **375px (iPhone SE), 768px (iPad), 1280px (Desktop)**.

Grid padrão:
```html
<!-- Cards: 1 col mobile, 2 tablet, 3 desktop -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
```

---

### Git — Commit Automático

Sempre que entregar alterações, incluir no README ou orientar o usuário a rodar:

```bash
# Ao criar o projeto
git init
git add .
git commit -m "feat: criação inicial do site [nome do cliente]"

# A cada alteração significativa
git add .
git commit -m "feat: adiciona seção de serviços"
git commit -m "fix: corrige menu mobile no iOS"
git commit -m "style: ajusta paleta de cores dark mode"
```

Padrão de commits: **Conventional Commits**
- `feat:` — nova funcionalidade/seção
- `fix:` — correção de bug
- `style:` — ajustes visuais
- `refactor:` — melhoria de código sem mudar comportamento
- `docs:` — atualização de README

---

## Seções Padrão de um Site Institucional

Ordem recomendada:

1. **Header/Nav** — Logo + menu + toggle dark/light
2. **Hero** — Headline forte + subtítulo + 2 CTAs (primário e secundário) + imagem/vídeo
3. **Sobre** — História, valores, diferenciais com ícones
4. **Serviços** — Cards com hover effect
5. **Números/Stats** — Contador animado (anos de experiência, clientes, projetos)
6. **Depoimentos** — Cards ou carrossel simples
7. **CTA intermediário** — Banner de conversão com botão pulsante
8. **Galeria ou Portfolio** — Se aplicável ao nicho
9. **FAQ** — Accordion simples
10. **Contato** — Formulário + mapa + informações de contato
11. **Footer** — Links rápidos + redes sociais + copyright + LGPD link

---

## Checklist Final (Antes de Entregar)

- [ ] Light mode funcionando corretamente
- [ ] Dark mode funcionando e sem elementos esquecidos
- [ ] Menu hamburger abre/fecha corretamente no mobile
- [ ] Drawer lateral sem double scrollbar
- [ ] AOS carregando e animando os elementos
- [ ] Scroll smooth funcionando
- [ ] Barra de scroll na cor do projeto
- [ ] Popup LGPD aparece no canto inferior direito após 2s
- [ ] Popup LGPD não reaparece após aceitar
- [ ] Botões com animação (pulse, hover scale ou shimmer)
- [ ] Hover nos cards e links de navegação
- [ ] Responsivo em mobile, tablet e desktop
- [ ] HTML semântico (sem `<div>` onde deveria ser `<section>`, `<nav>`, etc.)
- [ ] Fonte adequada ao nicho carregada via Google Fonts
- [ ] Commit feito com mensagem no padrão Conventional Commits
- [ ] Sem console.error no browser

---

## Observações Importantes

- **Nunca** usar dois scroll bars simultâneos. Sempre verificar se o drawer mobile usa `overflow-hidden` no body.
- **Nunca** usar fontes genéricas (Arial, Times New Roman) sem justificativa.
- AOS deve ter `once: true` para não reanimar ao subir a página.
- O toggle de dark/light deve persistir via `localStorage`.
- O popup LGPD deve aparecer apenas uma vez (verificar `localStorage`).
- Sempre testar o site com `prefers-color-scheme: dark` do sistema operacional como fallback.