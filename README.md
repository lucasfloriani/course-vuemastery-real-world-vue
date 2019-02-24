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

## Dynamic Routes

Usado para quando precisamos de url baseadas em valores dinâmicos, por exemplo acessar o perfil de um usuário específico utilizando seu username:

```js
// Toda a parte após : indica ao Vue Router que será totalmente dinâmico, ou seja, um parâmetro
{
  path: "/user/:username",
  name: "user",
  component: User,
}
```

Dentro do componente, podemos pegar este valor utilizando **\$route**, o qual representa o estado da rota atual, contendo várias informações da rota alem dos parâmetros.

```html
<h1>Usuário com username {{$router.params.username}}</h1>
```

Para criar um link para uma rota dinâmica usamos o seguindo código:

```html
<router-link
  :to="{
    name: 'user',
    params: {
      username: 'Gregg'
    }
  }"
>
  Gregg
</router-link>
```

Usar **\$route** dentro de um componente limita sua flexibilidade, existindo assim uma forma mais modular de fazer isto.
Podemos adicionar uma opção a mais em nossa rota para que os parametros que são passados na url sejam enviados ao componente como parâmetro:

```js
{
  path: "/user/:username",
  name: "user",
  component: User,
  props: true,
}
```

Assim os valores presentes em **\$route.params** são enviados como props ao componente adicionado na opção da rota.
Com isto o componente ficará mais maleável para utilização em vários locais, não ficando assim atrelado a url da rota onde será utilizado

## HTML5 History Mode

O modo padrão de simular páginas no Vue Router é o **Hashmode**, para remover-mos a **#** que aparece na url precisamos passar algumas configurações para a nossa instância do Router

```js
{
  mode: "history",
}
```

Esta opção usa o **HistoryMode** que utiliza a history API do navegador para mudar de página sem recarregar a página.

### Como funciona o History Mode

Diferente de Server Browsing onde toda url que é chamada é trazido um html diferente, com o History Mode toda url que é carregada traz o mesmo index.html

Porem quando carregamos Vue em um ambiente de produção diferente nós precisamos configurar para fazer isto funcionar.

Para configurar podemos encontrar um tutorial especifico na página do Vue demonstrando o passo a passo para deixar funcionando o **HTML5 History Mode**, forçando assim a sempre retornar o index.html, igual acontece normalmente no ambiente de desenvolvimento

Porem com isto seu servidor não responderá mais o erro 404. Podemos resolver este problema de várias maneiras, onde uma delas e adicionar uma rota com path **\*** por ultimo no seu router, para assim carregar uma página (componente) 404

OBS: _O History Mode não é utilizado como default pois somente suporta o IE versão 10 para frente e o Vue tenta suportar desde a versão 9_
