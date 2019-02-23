# Real World Vue

## Vue CLI

Interface de Linha de Comando para criar aplicações Vue, contendo um sistema completo para rápido desenvolvimento.

Para instala-lo, utilizamos o comando **npm i -g @vue/cli**

## Vue UI

Realize o mesmo que o vue cli, porem com interface web.

## Estrutura da aplicação

- **node_modules**: Pacotes que foram adicionados a aplicação;
- **public**: Lugar para adicionar arquivos que não seram alterados/usados pelo webpack como imagens;
- **src**: Onde o código da aplicação fica;
- **src/assets**: Arquivos como imagens, fontes, etc;
- **src/components**: Componentes/Blocks da aplicação Vue;
- **src/views**: Armazena arquivos para as diferentes páginas;
- **src/App.vue**: Root component, todos os componentes são derivados dele;
- **src/main.js**: Renderiza o App.vue e monta para o DOM;
- **src/router.js**: Arquivo que contem as rotas da aplicação;
- **src/store.js**: Arquivo da store do Vuex;
- **.gitignore**: Arquivos ignorados nos commits do git;
- **babel.config.js**: Configurações do babel;
- **package.json**: Configurações e pacotes adicionados a aplicação;

## Como a aplicação é carregada

No arquivo **main.js** é importado o Vue e o componente App.vue, sendo ele o principal da aplicação, onde criamos uma instancia Vue passando para ela renderizar o componente App e os middlewares (router e store) iniciando no elemento do dom pego pelo seletor passado no **\$mount**
Quando for iniciado, disponibiliza o diretório **public** e carrega o arquivo **index.html**

## Otimizando seu Editor (VSCode)

Instalar os plugins:

- **Vetur**: Syntax Highlighting, Code Snippets, Emmet for Vue
- **Copy Relative Path**: Copia o path relativo de um componente a outro
- **Vue VSCode Snippets (sarah.drasner)**: Snippets criados pelo time core do Vue, adicione a seguinte linha nas suas configurações de usuário do VSCode **"vetur.completion.useScaffoldSnippets": false**
- **Eslint**
- **Prettier**

Dicas:

- Com o comando **ctrl + p** é possível buscar o componente que deseja editar sem precisar colocar a mão no mouse
- Utilizando o Code Snippets do Vetur podemos criar novos componentes rapidamente, por exemplo, digitando scaffold e apertando enter ele irá criar a estrutura base para um componente Vue automaticamente.
- Para configurar o eslint com prettier vá no arquivo gerado do eslint (**.eslintrc.js**) e adicione na propriedade **extends** a seguinte string **'plugin:prettier/recommended'**. Pode-se adicionar tambem um arquivo com algumas configurações customizadas do prettier (**.prettierrc.js**).

Altere as configurações de usuário (User settings) do VSCode e adicione os seguintes configurações

```json
{
  "vetur.validation.template": false,
  "eslint.validate": [
    {
      "language": "vue",
      "autoFix": true
    },
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "javascript",
      "autoFix": true
    }
  ],
  "eslint.autoFixOnSave": true,
  "editor.formatOnSave": true
}
```

## Vue Router

Usado para criar Single Page Application (SPA), que são aplicativos web que carregam uma única página e dinâmicamente atualiza conforme o usuário interage com a aplicação.
Ao utilizar Client-Side Routing conseguimos criar uma SPA, algo que o Vue Router faz para nós.

Para criarmos links entre página utilizamos os seguintes componentes:

- **router-link**: Um componente que cria um link para navegar a uma rota
- **router-view**: Um placeholder o qual renderiza o componente encontrado com este link

Podemos utilizar tanto a url para ir em outras páginas como tambem o nome da rota utilizada

```js
// Rotas criadas
{
  path: "/",
  name: "home",
  component: Home
},
{
  path: "/about",
  name: "about",
  component: About
}
```

```html
<!-- Exemplo de utilização de rotas (nome e url) -->
<router-link :to="{ name: 'home' }">Home</router-link> |
<router-link to="/about">About</router-link>
```

O Benefício de utilizar rotas nomeadas é que se precisarmos trocar a url futuramente, só precisamos alterar em um local (Arquivo de rotas)

Quando uma aplicação já está no ar e queremos alterar a url de uma das páginas, podemos solucionar de várias maneiras, sendo elas:

Exemplos: **about** para **about-us**

Por Redirect (Melhor para SEO)

```js
{
  path: "/about-us",
  name: "about",
  component: About
},
{
  path: "/about",
  redirect: { name: "about" }
  // redirect: "/about-us"
}
```

Por Alias

```js
{
  path: "/about-us",
  name: "about",
  component: About,
  alias: "/about"
}
```

É possível criar tambem lazy load pages, onde o código da página é baixado somente quando ela for acessada:

```js
{
  path: "/about",
  name: "about",
  // route level code-splitting
  // this generates a separate chunk (about.[hash].js) for this route
  // which is lazy-loaded when the route is visited.
  component: () =>
    import(/* webpackChunkName: "about" */ "./views/About.vue")
}
```
