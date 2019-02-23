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
