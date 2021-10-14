# web-template

Este repo contém a sequência explicada de criação de um ambiente de desenvolvimento para projetos web. O resultado virou um _template_ para projetos web, com ferramentas pré-configuradas, disponível em https://github.com/ermogenes/web-template. Use a vontade, ou crie o seu a partir das informações abaixo.

## Versionamento

Iniciaremos com o estabelecimento de que nosso código será versionado. Utilizaremos o [Git](https://git-scm.com/) para isso. Podemos iniciar o versionamento com `git init`, mas como queremos armazenar em um repositório remoto, vamos utilizar o [GitHub](https://github.com/) e baixar o repositório.

Vamos criar um repositório remoto para armazenar nosso código.

![](imagens/git-01.png)

Ele será criado como _template_, mas isso não é necessário.

![](imagens/git-02.png)

Baixe seu código para que possamos iniciar, utilizando o URL disponibilizado na página do repositório.

![](imagens/git-03.png)

Nesse caso, o URL para clonagem é `https://github.com/ermogenes/web-template.git`.

Abra uma pasta para trabalho no terminal, e faça a clonagem do repositório.

```sh
git clone https://github.com/ermogenes/web-template.git
```

Faça a configuração do repositório para que ele não mude o caractere de final de linha dos arquivos ao salvar, usando:

```sh
git config core.autocrlf false
```

Acesse e abra essa pasta no seu editor preferido. Nesse exemplo, utilizaremos o [VsCode](https://code.visualstudio.com/).

```sh
code .
```

## Configurações do editor

### Extensões

Vamos indicar as extensões recomendadas para quem abrir o projeto. Essa indicações facilitam a instalação para quem abrir o _template_.

Fazemos isso usando essa opção _Add to Workspace Recommendations_:

![](imagens/vscode-01.png)

Serão adicionadas entradas em `.vscode/extensions.json`.

No nosso caso, adicionaremos três recomendações, que serão utilizadas na sequência.

- [EditorConfig (editorconfig.editorconfig)](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
- [Prettier (esbenp.prettier-vscode)](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [ESLint (dbaeumer.vscode-eslint)](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

Ficará assim:

```json
{
  "recommendations": [
    "editorconfig.editorconfig",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode"
  ]
}
```

Elas aparecerão para todos os usuários:

![](imagens/vscode-02.png)

Não é necessário instalar ainda. Faremos isso a cada passo.

### Configurações entre diferentes editores

Usaremos o [EditorConfig](https://editorconfig.org/) para indicar a configuração recomendada, independentemente do editor. Ele é suportado nativamente por alguns, e através de plugins por praticamente todos.

Criaremos uma configuração de referência, e ela será seguida pelo VsCode através da extensão do EditorConfig.

A configuração ficará no arquivo `.editorconfig`, na raiz do projeto:

```ini
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

Para o VsCode é necessária a instalação da [extensão do EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig).

## Gerenciamento de pacotes

Vamos usar o [npm](https://www.npmjs.com/) para baixar componentes no nosso projeto. Além disso, nos dará a capacidade de configuração e execução de _scripts_, que facilitam bastante o desenvolvimento. Podemos instalá-lo diretamente, ou utilizar uma versão incorporada na instalação do [Node.js](https://nodejs.org/).

O npm é incluído em um projeto através da criação do arquivo `package.json`. Podemos criá-lo interativamente usando `npm init`, mas vamos deixar o npm criá-lo com os valores necessários a cada passo.

Por exemplos, vamos instalar os pacotes do webpack:

```sh
npm install --save-dev webpack webpack-cli
```

![](imagens/npm-01.png)

Duas coisas serão criadas:

- um arquivo `package.json` (bem como seu par `package-lock.json`), onde estão indicadas as configurações do pacote;
- uma pasta `node_modules`, onde serão instaladas as dependências.

Em `package.json`:

```json
{
  "devDependencies": {
    "webpack": "^5.58.2",
    "webpack-cli": "^4.9.0"
  }
}
```

Na seção `devDependencies` ficam as indicações dos pacotes que são dependências de desenvolvimento, ou seja, só são necessárias no ambiente de desenvolvimento, mas não em produção.

A pasta `node_modules` não deve ser versionada, pois sempre pode ser baixada novamente a partir das indicações em `package.json` usando o comando `npm install` (ou `npm ci`).

Podemos, então, adicioná-la ao arquivo `.gitignore` na raiz do projeto, que ficará assim:

```
node_modules/
```

## Bundling (building)

Vamos configurar o [webpack](https://webpack.js.org/) para ser a nossa ferramenta de _bundling_, que é o equivalente ao _build_ para projetos JavaScript (e web em geral).

De maneira geral, seu código será a entrada, e o processo de build gerará um código modificado, otimizado para execução.

_A instalação foi realizada no passo acima._

Vamos iniciar a configuração do webpack interativamente, usando o `webpack-cli`:

```sh
npx webpack init
```

Ele iniciará instalando o gerador `@webpack-cli/generators`. Depois fará diversas perguntas para criar a configuração equivalente. Nesse exemplo, usei:

- _Which of the following JS solutions do you want to use?_ **ES6**: JavaScript para a web.
- _Do you want to use webpack-dev-server?_ **Yes**: Servidor web de desenvolvimento.
- _Do you want to simplify the creation of HTML files for your bundle?_ **Yes**: Tratamento de arquivos HTML.
- _Do you want to add PWA support?_ **No**: Suporte a PWA desativado.
- _Which of the following CSS solutions do you want to use?_ **SASS**: Suporte a SASS.
- _Will you be using CSS styles along with SASS in your project?_ **Yes**: Suporte a CSS sem SASS.
- _Will you be using PostCSS in your project?_ **Yes**: Processamento de CSS com PostCSS.
- _Do you want to extract CSS for every file?_ **Yes**: Extração de CSS de todos os arquivos.
- _Do you like to install prettier to format generated configuration?_ **Yes**: suporte a Prettier.

Confirme a substituição de `package.json`, e dos demais arquivos se necessário.

Veja como o arquivo ficou:

```json
{
  "devDependencies": {
    "@babel/core": "^7.15.8",
    "@babel/preset-env": "^7.15.8",
    "@webpack-cli/generators": "^2.4.0",
    "autoprefixer": "^10.3.7",
    "babel-loader": "^8.2.2",
    "css-loader": "^6.4.0",
    "html-webpack-plugin": "^5.3.2",
    "mini-css-extract-plugin": "^2.4.2",
    "postcss": "^8.3.9",
    "postcss-loader": "^6.2.0",
    "prettier": "^2.4.1",
    "sass": "^1.42.1",
    "sass-loader": "^12.2.0",
    "style-loader": "^3.3.0",
    "webpack": "^5.58.2",
    "webpack-cli": "^4.9.0",
    "webpack-dev-server": "^4.3.1"
  },
  "version": "1.0.0",
  "description": "My webpack project",
  "name": "my-webpack-project",
  "scripts": {
    "build": "webpack --mode=production --node-env=production",
    "build:dev": "webpack --mode=development",
    "build:prod": "webpack --mode=production --node-env=production",
    "watch": "webpack --watch",
    "serve": "webpack serve"
  }
}
```

Foram adicionadas muitas dependências de desenvolvimento, como compilador de JavaScript [Babel](https://babeljs.io/) interno ao webpack, diversos _loaders_ que extraem código dos arquivos, diversos _plugins_ que transformam esses códigos durante o processo de _build_, e as ferramentas [Prettier](https://prettier.io/) (que permite formatar o código utilizando estilos de codificação), [Sass](https://sass-lang.com/) (que adiciona funcionalidades ao CSS) e [PostCSS](https://postcss.org/) (um transpilador de CSS para geração de código otimizado). Também é criados arquivos de entrada da compilação em `./src/index.js` e `index.html`.

São criados `scripts` que podem ser executados usando `npm run <nome do script>`:

- `npm run build`: realiza o processo de _build_ do webpack, para produção.
- `npm run build:dev`: realiza o processo de _build_ do webpack, para desenvolvimento (mais rápido porém menos otimizado).
- `npm run build:prod`: como em `npm run build`.
- `npm run watch`: inicia um _worker_ para monitorar os arquivos e recompilá-los quando necessário.
- `npm run serve`: inicia um servidor web para desenvolvimento.

Podemos criar quantos _scripts_ necessitarmos, sempre com o nome e o comando a ser executado (que pode ser qualquer coisa que o seu terminal possa executar).

O arquivo `.babelrc` contém as configurações do Babel.

Em `webpack.config.js`, teremos todas as configurações do processo do webpack. Esse arquivo é escrito em JavaScript, e o alteraremos com frequência.

## Customização do _build_

Vamos ajustar o ponto de entrada para a pasta `src`, tanto no JavaScript quanto no HTML. Vamos mover `index.html` para `src` e ajustar a configuração em `webpack.config.js`.

```json
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "./src/index.html"),
    }),
```

A entrada JavaScript já está correta. O caminho de saída está configurado para a pasta `dist`. Vamos deixá-la assim, mas devemos incluir a entrada `dist/` em `.gitignore`.

Assim, a cada _build_ os arquivos `./src/index.js` e `./src/index.html` serão processados, bem como todos os conteúdos referenciados, e a saída será gravada em `./dist/`, que não deve ser versionada, mas será o único conteúdo a ser publicado no seu servidor web de produção.

Vamos adicionar três processos ao nosso build:

- `clean-webpack-plugin`, que apaga todos os arquivos da pasta destino antes do _build_, evitando a manutenção de sujeira de execuções anteriores;
- `html-loader`, que inclui arquivos HTML adicionais no processo;
- `css-minimizer-webpack-plugin`, que minifica os arquivos CSS gerados.

Vamos instalar os pacotes:

```sh
npm install --save-dev clean-webpack-plugin html-loader css-minimizer-webpack-plugin
```

E realizar as configurações em `webpack.config.js`:

- Criaremos objetos para os _plugins_;
- Adicionaremos o _plugin_ `CleanWebpackPlugin` na lista de _plugins_;
- Adicionaremos uma regra para processamento de HTML;
- Adicionaremos a otimização com `CssMinimizerPlugin`.

```js
// ...
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
// ...
  plugins: [
    // ...
    new CleanWebpackPlugin(),
    // ...
  ],
// ...
  module: {
    rules: [
    // ...
      {
        test: /\.(html)$/,
        use: "html-loader",
      },
    ],
    // ...
  },
  optimization: {
    minimizer: ["...", new CssMinimizerPlugin()],
  },
// ...
```

Vamos também configurar o PostCSS para utilizar [_postcss-preset-env_](https://github.com/csstools/postcss-preset-env), que moderniza o código CSS gerado. Por padrão, o PostCSS usa o _plugin_ `autoprefixer`, mas vamos substituí-lo por este, que inclui o anterior e muito mais.

```sh
npm install --save-dev postcss-preset-env
```

E depois alteraremos `postcss.config.js`:

```js
// ...
  plugins: [["postcss-preset-env"]],
// ...
```

## Formatação de código

Vamos configurar o [Prettier](https://prettier.io/). Ele será utilizado para formatar nosso código de acordo com regras de estilo, a cada salvamento (ou a qualquer tempo, manualmente). Não vamos incuí-lo no processo de _build_, mas vamos utilizá-lo de forma integrada ao editor VsCode, através da sua [extensão](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode).

O pacote já foi incluído na criação do projeto webpack, mas pode ser feito usando `npm install --save-dev prettier`.

Para que ele seja executada automaticamente ao salvar arquivos no VsCode, precisamos fazer a configuração abaixo:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

Colocando esse conteúdo em um arquivo `./.vscode/settings.json`, ele valerá para todos que abrirem esse repositório no VsCode.

Para configurar o Prettier, criamos na raiz o arquivo `.prettierrc` contendo:

```json
{
  "trailingComma": "es5",
  "singleQuote": true
}
```

Agora, ao salvar, serão incluídos `;` ao final de cada linha, e `"` será substituído por `'`. Há outras configurações possíveis.

Para garantir, vamos criar `.prettierignore` para marcar os arquivos que não devem ser processados:

```
node_modules/
dist/
```

Podemos abrir cada arquivo e salvá-los novamente, ou processá-los em lote nesse primeiro momento, usando:

Para somente verificar:

```sh
npx prettier --check .
```

Para corrigir:

```sh
npx prettier --write .
```

Vamos adicionar esses comandos como scripts no npm em `package.json`:

```json
// ...
    "prettier:check": "npx prettier --check .",
    "prettier": "npx prettier --write .",
// ...
```

Agora podemos usar `npm run prettier` e `npm run prettier:check`, quando necessário.

## _Linting_

O processo de _bundling_ não inclui a verificação de código contra erros ou más-práticas. Para isso, utilizaremos o [ESLint](https://eslint.org/).

Vamos instalar o ESLint e o _plugin_ do webpack:

```sh
npm install --save-dev eslint eslint-webpack-plugin
```

Criaremos a configuração do ESLint usando:

```sh
npx eslint --init
```

- _How would you like to use ESLint?_ **style** : configura para encontrar erros, corrigí-los e forçar o uso de estilos de codificação.
- _What type of modules does your project use?_ **esm** : estilo de importação de módulos.
- _Which framework does your project use?_ **none** : não usa nenhum _framework_ por padrão, como React ou Vue.
- _Does your project use TypeScript?_ **No** : não usa TypeScript.
- _Where does your code run?_ **browser** : código para navegadores, e não Node.js.
- _How would you like to define a style for your project?_ **guide** : permite escolher um estilo de codificação.
- _Which style guide do you want to follow?_ **airbnb** : estilo de codificação do [Airbnb](https://github.com/airbnb/javascript).
- _What format do you want your config file to be in?_ **JavaScript** : formato do arquivo de configuração.
- _Would you like to install them now with npm?_ **Yes** : instalação das dependências.

Será criado `.eslintrc.js`, onde podemos, por exemplo, ignorar algumas regras.

Por exemplo, o estilo selecionado não permite o uso de `console.log` em páginas web. Para permitir, podemos adicionar a seguinte regra:

```js
  rules: {
    'no-console': 'off', // desliga verificação [no-console]
  },
```

Vamos indicar os arquivos que não devem ser verificados criando `.eslintignore`:

```
node_modules/
dist/
```

Vamos configurar o webpack para incluir o _linting_ na processo de _build_, alterando `webpack.config.js`:

```js
// ...
const ESLintPlugin = require('eslint-webpack-plugin');
// ...
  plugins: [
    // ...
    new ESLintPlugin(),
    // ...
  ],
// ...
  optimization: {
    minimizer: [
      '...',
      // ...
      new ESLintPlugin()
    ],
  },
  // ...
```

A instalação da [extensão do ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) permite que o VsCode exiba os erros já durante o tempo de codificação.

Digno de nota, o ESLint aponta um erro em `webpack.config.js`, na linha:

```js
const isProduction = process.env.NODE_ENV == 'production'; // use '===' em vez de '=='
```

## Testando o _template_

Para testar e treinar as funcionalidades, pode-se utilizar os arquivos de https://github.com/ermogenes/web-template-src-teste.

## Iniciando um projeto usando o _template_

Crie um repositório no GitHub usando este _template_.

Clone ele em seu ambiente.

Baixe as dependências:

```sh
npm install
```

Execute o servidor local:

```sh
npm run serve
```

Publique a versão de produção, em `./dist/`:

```sh
npm run build
```
