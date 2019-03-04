# Babel boyicha qo'llanma

Bu qo'llanma [Babel](https://babeljs.io) bo'yicha barcha kerakli narsalarni o'z ichiga oladi.

[![cc-by-4.0](https://licensebuttons.net/l/by/4.0/80x15.png)](http://creativecommons.org/licenses/by/4.0/)

Bu qo'llanma boshqa tillarda ham mavjud, [README](/README.md) da boshqa tillarda korishingiz mumkin.

# Mudarija

- [Kirish](#toc-introduction)
- [Babel ni sozlash](#toc-setting-up-babel)
  - [`babel-cli`](#toc-babel-cli)
    - [Babel CLI ni proyekt ichiga qoyish](#toc-running-babel-cli-from-within-a-project)
  - [`babel-register`](#toc-babel-register)
  - [`babel-node`](#toc-babel-node)
  - [`babel-core`](#toc-babel-core)
- [Babel ni sozlash](#toc-configuring-babel)
  - [`.babelrc`](#toc-babelrc)
  - [`babel-preset-es2015`](#toc-babel-preset-es2015)
  - [`babel-preset-react`](#toc-babel-preset-react)
  - [`babel-preset-stage-x`](#toc-babel-preset-stage-x)
- [Babel generate qilgan kodni ishga tushirish](#toc-executing-babel-generated-code)
  - [`babel-polyfill`](#toc-babel-polyfill)
  - [`babel-runtime`](#toc-babel-runtime)
- [Babel ni sozlash (Murakkab darajada)](#toc-configuring-babel-advanced)
  - [Pluginlarni qo'lda sozlash](#toc-manually-specifying-plugins)
  - [Plugin optsiyalari](#toc-plugin-options)
  - [environment ga qarab Babel ni moslashtirish](#toc-customizing-babel-based-on-environment)
  - [preset yasash](#toc-making-your-own-preset)
- [Babel va boshqa toollar](#toc-babel-and-other-tools)
  - [Statik analiz toollari](#toc-static-analysis-tools)
    - [Linting](#toc-linting)
    - [Code Stili](#toc-code-style)
    - [Dokumentatsiya](#toc-documentation)
  - [Frameworklar](#toc-frameworks)
    - [React](#toc-react)
  - [Matn muharirlari va IDE lar](#toc-text-editors-and-ides)
- [Babel boyicha yordam](#toc-babel-support)
  - [Babel forumlar](#toc-babel-forum)
  - [Babel chatlar](#toc-babel-chat)
  - [Babel muammolari](#toc-babel-issues)
    - [Babel haqida bug yozish](#toc-creating-an-awesome-babel-bug-report)

# <a id="toc-introduction"></a>Kirish

Babel universal kop funksionalli JavaScript kompilatori.
Babel ni ishlatib, siz oxirgi JavaScript imkoniyaylaridan foydalana olasiz yoki yarata olasiz.
 
JavaScript juda tez rivojlanayotgan va kop imkoniyatlar qoshiladigan til hisoblanadi.
Babel ni ishlatgan holda, siz JavaScriptning oxirgi imkoniyatlarni, hali ular rasmiy elon
qilinmasidan oldin foydalana olasiz.

Babel bu imkoniyatni quyidagi holda amalga oshiradi. JavaScriptnig oxirigi standtarlarida yozilgan kodni
barcha joyda ishlaydigan JavaScript versiyasiga ogiradi. Bu jarayon source-to-source kompilatsiya deyiladi
va transpiling ham deb nomlanadi.

Misol uchun, Babel yangi ES2015 arrow funksiya sintaksisini bu ko'rinishdan:

```js
const square = n => n * n;
```

Bu korinishga o'gira oladi:

```js
const square = function square(n) {
  return n * n;
};
```

Babel bundan ham koproq imkoniyatlarga ega. Babelda React uchun JSX sintaksisi 
va statik tip tekshirish uchun Flow sintaksisi imkoniyatlari mavjud, 

Qo'shimchasiga, Babel oddiy plugin lardan iborat. Har kim Babel uchun o'zini 
pluginini yaratishi va Babelning to'liq imkoniyatlaridan foydalanib hohlagan 
amalarni bajarish mumkin.

*Yana qo'shimcha qilib* Babel birnechta modullarga bo'linadi. Bu moddullarni ishlatgan holda,
JavaScript uchun keyingi avlod toollarini yaratish mumkin.

Ko'plar bu imkoniyatlardan foydalanadi va shu tufayli Babelning ekosistemasi juda katta va xilma xil.
Bu qo'llanma orqali, men Babelning ichgi ishlash mexanizmini va qolgan komunitylardagi toollar ishlashini
tushuntirib beraman.

> ***Oxirgi yangiliklardan xabardor bo'lish uchun, [@thejameskyle](https://twitter.com/thejameskyle)
> ni Twitterda follow qiling.***

----

# <a id="toc-setting-up-babel"></a>Babel ni sozlash

JavaScript komunitiysida juda ko'p frameworklar, platformlar, build toollar bo'lgani uchun, Babelda
ularning barchasi uchun integratsiya qilish imkoniyati mavjud. Gulp dan boshlab Browserify gacha, Ember
dan boshlab Meteor gacha va boshqa barcha toollar bilan integratsiya imkoni mavjud.

Bu qo'llanmada biz faqat Babelni ozini so'zlashni ko'rib chiqamiz. Boshqa integratsiyalar
uchun [setup page](http://babeljs.io/docs/setup) da tanishingiz mumkin.

> **Eslatma:** Bu qo'llanmada `node` va `npm` command line toollaridan foydalaniladi.
> Boshlashdan oldin bu toollar bilan tanishib chiqing.

## <a id="toc-babel-cli"></a>`babel-cli`

Babel CLI bilan kommand line orqali fayllarni  kompilatsiya qilish juda onson.

Babel CLI ni globalniy o'rnatib, asosiy amallarni ko'rib chiqamiz:

```sh
$ npm install --global babel-cli
```

Endi faylni quyidagicha kompilatsiya qilamiz:

```sh
$ babel my-file.js
```

Bunda kompilatsiya qilingan kod terminalda ko'rinadi. Buni faylga yozish uchun
`--out-file` yoki `-o` parametrini ko'rsatamiz.

```sh
$ babel example.js --out-file compiled.js
# yoki
$ babel example.js -o compiled.js
```

Butun papkani yangi papkaga kompilatsiya qilish uchun `--out-dir` yoki `-d` parametrlarini
ko'rsatamiz.

```sh
$ babel src --out-dir lib
# yoki
$ babel src -d lib
```

### <a id="toc-running-babel-cli-from-within-a-project"></a>Babel CLI ni proyekt ichida ishlatish

Babel CLI ni globalniy o'rnatish imkoniyati bor. Biroq Babel CLI ni har bita proyektga 
alohida **lokalniy** o'rnatish taklif etiladi.

Bunga ikkita asosiy sabablar mavjud:

   1. Har xil proyektda Babelning versiyasi har xil bo'ishi mumkin va har birini alohida 
      yangilash imkoniyati mavjud.
   2. Yana bitta foydasi shuki, Babel CLI sizning ishlayotgan environmentga bog'liq bo'lmaydi.
      Bu bilan proyektni o'ranitishingiz va boshqa joylarda ishlatishingiz onsonroq bo'ladi.

Babel CLI ni lokal o'rnatish quyidagicha amalga oshiriladi:

```sh
$ npm install --save-dev babel-cli
```

> **Eslatma:** Babelni global o'rnatish tavsiya etilmagani uchun, quyidagi komanda
> orqali uni o'chirishingiz mumkin:
>
> ```sh
> $ npm uninstall --global babel-cli
> ```

Babel CLI ni o'rnatganingizdan so'ng, sizning `package.json` faylingiz quyidagi
ko'rinishda bo'lishi kerak:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "devDependencies": {
    "babel-cli": "^6.0.0"
  }
}
```

Babelni to'g'ridan to'g'ri komand line orqali ishlatmasdan, uni **npm scripts** ichiga 
joylashtiramiz. **npm scripts** ichidagi kommandalar lokal o'rnatilgan Babelni ishlatadi.

`package.json` fayl ichiga `"scripts"` ni qo'shing va babel komandasini `build` ichiga
joylashtiring. 

Quyidagicha:

```diff
  {
    "name": "my-project",
    "version": "1.0.0",
+   "scripts": {
+     "build": "babel src -d lib"
+   },
    "devDependencies": {
      "babel-cli": "^6.0.0"
    }
  }
```

Endi terminal orqali quyidagi komandani ishga tushuramiz:

```js
npm run build
```

Bu komanda oldingi Babel komandalaridaka ishlaydi, faqat bunda lokal o'rnatilgan
Babel ishga tushadi.

## <a id="toc-babel-register"></a>`babel-register`

Babelni ishlatishni keyingi eng ko'p tarqalgan usul bu Babelni `babel-register` 
orqali ishlatishdir. Bunda fayllarni require qilib Babelni ishga tushurishingiz
mumkin.

Bu usulni productionda ishlatish tavsiya etilmaydi. Bu usul bilan kodni deploy qilish
bad practise hisoblanadi. Deploy qilishdan oldin kompilatsiya qilish tavsiya etiladi.
Biroq lokal fayllarni yoki build scriptlarni ishga tushurishda bu usul juda yordam beradi.

Birinchi bo'lib `index.js` faylni yaratamiz.

```js
console.log("Hello world!");
```

Bu kodni `node index.js` orqali ishga tushursak, unda Babelga kompilatsiya bo'lmaydi. 
Shuning uchun `babel-register` ni sozlaymiz.

Birinchi `babel-register` ni o'rnating.

```sh
$ npm install --save-dev babel-register
```

Keyin `register.js` fayl yarating va unga quyidagi kodni yozing:

```js
require("babel-register");
require("./index.js");
```

Bunda Babel, Node ning module sistemasiga **registratsiya** bo'ladi va barcha
`require` orqali chaqirilgan fayllar kompilatsiya bo'ladi.

Endi `node index.js` ni o'rniga `register.js` ni ishga tushursak boladi.

```sh
$ node register.js
```

> **Eslatma:** Kompilatsiya qilinaytgan faylda Babelni registratsiya qilish imkoniyati
> yo'q. Bunda Babel kompilatsiya qilishidan oldin, birinchi bo'lib Node faylni ishga tushuradi.
>
> ```js
> require("babel-register");
> // kompilatsiya bo'lmaydi:
> console.log("Hello world!");
> ```

## <a id="toc-babel-node"></a>`babel-node`

Agarda kodni faqat `node` CLI orqali ishga tushursangiz, unda Babel ni
ishlatishning eng onson yo'li bu `babel-node` CLI bilan ishlatish hisoblanadi.

Bu usulni productionda ishlatish tavsiya etilmaydi. Bu usul bilan kodni deploy qilish
bad practise hisoblanadi. Deploy qilishdan oldin kompilatsiya qilish tavsiya etiladi.
Biroq lokal fayllarni yoki build scriptlarni ishga tushurishda bu usul juda yordam beradi.


Birinchi bo'lib `babel-cli` o'rnating:

```sh
$ npm install --save-dev babel-cli
```

> **Eslatma:** `babel-node` ni lokal o'rnatish sababini
> [Babel CLI ni proyekt ichiga qoyish](#toc-running-babel-cli-from-within-a-project)
> bo'limida ko'rishingiz mumkin.

Keyin, `node` bilan ishga tushuriladigan kommadalarni `babel-node` ga almashtiring.

Agarda npm `scripts` ni ishlatsangiz, unda quyidagi ko'rinishda yozishingiz mumkin:

```diff
  {
    "scripts": {
-     "script-name": "node script.js"
+     "script-name": "babel-node script.js"
    }
  }
```

Aks holda `babel-node`ning path ni ko'ratishingizga to'g'ri keladi.

```diff
- node script.js
+ ./node_modules/.bin/babel-node script.js
```

> Maslahat: [`npm-run`](https://www.npmjs.com/package/npm-run) ni ishlatsangiz ham bo'ladi.

## <a id="toc-babel-core"></a>`babel-core`

Agarda siz fayllarinigizni o'zingiz transpile qilmmoqchi bo'lasngiz unda `babel-core` ni
o'zini ishlatishingiz mumkin.

Birinchi bo'lib `babel-core`ni o'rnating.

```sh
$ npm install babel-core
```

```js
var babel = require("babel-core");
```

`babel.transform` orqali JavaScript kodni to'g'ridan to'g'ri kompilatsiya qilishingiz
mumkin. 

```js
babel.transform("code();", options);
// => { code, map, ast }
```

Agarda fayllar bilan ishlasangiz unda asinxron api larni ishlatshingiz mumkin:

```js
babel.transformFile("filename.js", options, function(err, result) {
  result; // => { code, map, ast }
});
```

Yoki sinxron api bilan:

```js
babel.transformFileSync("filename.js", options);
// => { code, map, ast }
```

`babel.transformFromAst` bilan Babel AST ni to'g'ridan to'g'ri kodga o'gira
olasiz.

```js
babel.transformFromAst(ast, code, options);
// => { code, map, ast }
```

Yuqoridagi metodlardagi `options` haqida malumotni shu linkda korishingiz mumkin
 https://babeljs.io/docs/usage/api/#options.

----

# <a id="toc-configuring-babel"></a>Babelni sozlash

Babel faqatgina JavaScript fayllarni bir joydan ikkinchi joyga kochirish
uchun ishlatilmaydi, balkim undan ham ko'p narsalarga qodir.

Biz hali Babelni to'liq kuchidan foydalanmadik.

> Since Babel is a general purpose compiler that gets used in a myriad of
> different ways, it doesn't do anything by default. You have to explicitly tell
> Babel what it should be doing.

You can give Babel instructions on what to do by installing **plugins** or
**presets** (groups of plugins).

## <a id="toc-babelrc"></a>`.babelrc`

Before we start telling Babel what to do. We need to create a configuration
file. All you need to do is create a `.babelrc` file at the root of your
project. Start off with it like this:

```js
{
  "presets": [],
  "plugins": []
}
```

This file is how you configure Babel to do what you want.

> **Note:** While you can also pass options to Babel in other ways the
> `.babelrc` file is convention and is the best way.

## <a id="toc-babel-preset-es2015"></a>`babel-preset-es2015`

Let's start by telling Babel to compile ES2015 (the newest version of the
JavaScript standard, also known as ES6) to ES5 (the version available in most
JavaScript environments today).

We'll do this by installing the "es2015" Babel preset:

```sh
$ npm install --save-dev babel-preset-es2015
```

Next we'll modify our `.babelrc` to include that preset.

```diff
  {
    "presets": [
+     "es2015"
    ],
    "plugins": []
  }
```

## <a id="toc-babel-preset-react"></a>`babel-preset-react`

Setting up React is just as easy. Just install the preset:

```sh
$ npm install --save-dev babel-preset-react
```

Then add the preset to your `.babelrc` file:

```diff
  {
    "presets": [
      "es2015",
+     "react"
    ],
    "plugins": []
  }
```

## <a id="toc-babel-preset-stage-x"></a>`babel-preset-stage-x`

JavaScript also has some proposals that are making their way into the standard
through the TC39's (the technical committee behind the ECMAScript standard)
process.

This process is broken through a 5 stage (0-4) process. As proposals gain more
traction and are more likely to be accepted into the standard they proceed
through the various stages, finally being accepted into the standard at stage 4.

These are bundled in babel as 4 different presets:

- `babel-preset-stage-0`
- `babel-preset-stage-1`
- `babel-preset-stage-2`
- `babel-preset-stage-3`

> Note that there is no stage-4 preset as it is simply the `es2015` preset
> above.

Each of these presets requires the preset for the later stages. i.e.
`babel-preset-stage-1` requires `babel-preset-stage-2` which requires
`babel-preset-stage-3`.

Simply install the stage you are interested in using:

```sh
$ npm install --save-dev babel-preset-stage-2
```

Then you can add it to your `.babelrc` config.

```diff
  {
    "presets": [
      "es2015",
      "react",
+     "stage-2"
    ],
    "plugins": []
  }
```

----

# <a id="toc-executing-babel-generated-code"></a>Executing Babel-generated code

So you've compiled your code with Babel, but this is not the end of the story.

## <a id="toc-babel-polyfill"></a>`babel-polyfill`

Almost all futuristic JavaScript syntax can be compiled with Babel, but the same
is not true for APIs.

For example, the following code has an arrow function that needs to be compiled:

```js
function addAll() {
  return Array.from(arguments).reduce((a, b) => a + b);
}
```

Which turns into this:

```js
function addAll() {
  return Array.from(arguments).reduce(function(a, b) {
    return a + b;
  });
}
```

However, this still won't work everywhere because `Array.from` doesn't exist
in every JavaScript environment.

```
Uncaught TypeError: Array.from is not a function
```

To solve this problem we use something called a
[Polyfill](https://remysharp.com/2010/10/08/what-is-a-polyfill). Simply put, a
polyfill is a piece of code that replicates a native api that does not exist in
the current runtime. Allowing you to use APIs such as `Array.from` before they
are available.

Babel uses the excellent [core-js](https://github.com/zloirock/core-js) as its
polyfill, along with a customized
[regenerator](https://github.com/facebook/regenerator) runtime for getting
generators and async functions working.

To include the Babel polyfill, first install it with npm:

```sh
$ npm install --save babel-polyfill
```

Then simply include the polyfill at the top of any file that requires it:

```js
import "babel-polyfill";
```

## <a id="toc-babel-runtime"></a>`babel-runtime`

In order to implement details of ECMAScript specs, Babel will use "helper"
methods in order to keep the generated code clean.

Since these helpers can get pretty long, and they get added to the top of every
file you can move them into a single "runtime" which gets required.

Start by installing `babel-plugin-transform-runtime` and `babel-runtime`:

```sh
$ npm install --save-dev babel-plugin-transform-runtime
$ npm install --save babel-runtime
```

Then update your `.babelrc`:

```diff
  {
    "plugins": [
+     "transform-runtime",
      "transform-es2015-classes"
    ]
  }
```

Now Babel will compile code like the following:

```js
class Foo {
  method() {}
}
```

Into this:

```js
import _classCallCheck from "babel-runtime/helpers/classCallCheck";
import _createClass from "babel-runtime/helpers/createClass";

let Foo = function () {
  function Foo() {
    _classCallCheck(this, Foo);
  }

  _createClass(Foo, [{
    key: "method",
    value: function method() {}
  }]);

  return Foo;
}();
```

Rather than putting the `_classCallCheck` and `_createClass` helpers in every
single file where they are needed.

----

# <a id="toc-configuring-babel-advanced"></a>Configuring Babel (Advanced)

Most people can get by using Babel with just the built-in presets, but Babel
exposes much finer-grained power than that.

## <a id="toc-manually-specifying-plugins"></a>Manually specifying plugins

Babel presets are simply collections of pre-configured plugins, if you want to
do something differently you manually specify plugins. This works almost exactly
the same way as presets.

First install a plugin:

```sh
$ npm install --save-dev babel-plugin-transform-es2015-classes
```

Then add the `plugins` field to your `.babelrc`.

```diff
  {
+   "plugins": [
+     "transform-es2015-classes"
+   ]
  }
```

This gives you much finer grained control over the exact transforms you are
running.

For a full list of official plugins see the
[Babel Plugins page](http://babeljs.io/docs/plugins/).

Also take a look at all the plugins that have been
[built by the community](https://www.npmjs.com/search?q=babel-plugin). If you
would like to learn how to write your own plugin read the
[Babel Plugin Handbook](plugin-handbook.md).

## <a id="toc-plugin-options"></a>Plugin options

Many plugins also have options to configure them to behave differently. For
example, many transforms have a "loose" mode which drops some spec behavior in
favor of simpler and more performant generated code.

To add options to a plugin, simply make the following change:

```diff
  {
    "plugins": [
-     "transform-es2015-classes"
+     ["transform-es2015-classes", { "loose": true }]
    ]
  }
```

> I'll be working on updates to the plugin documentation to detail every option
> in the coming weeks.
> [Follow me for updates](https://twitter.com/thejameskyle).

## <a id="toc-customizing-babel-based-on-environment"></a>Customizing Babel based on environment

Babel plugins solve many different tasks. Many of them are development tools
that can help you debugging your code or integrate with tools. There are also
a lot of plugins that are meant for optimizing your code in production.

For this reason, it is common to want Babel configuration based on the
environment. You can do this easily with your `.babelrc` file.

```diff
  {
    "presets": ["es2015"],
    "plugins": [],
+   "env": {
+     "development": {
+       "plugins": [...]
+     },
+     "production": {
+       "plugins": [...]
+     }
    }
  }
```

Babel will enable configuration inside of `env` based on the current
environment.

The current environment will use `process.env.BABEL_ENV`. When `BABEL_ENV` is
not available, it will fallback to `NODE_ENV`, and if that is not available it
will default to `"development"`.

**Unix**

```sh
$ BABEL_ENV=production [COMMAND]
$ NODE_ENV=production [COMMAND]
```

**Windows**

```sh
$ SET BABEL_ENV=production
$ [COMMAND]
```

> **Note:** `[COMMAND]` is whatever you use to run Babel (ie. `babel`,
> `babel-node`, or maybe just `node` if you are using the register hook).
>
> **Tip:** If you want your command to work across unix and windows platforms
> then use [`cross-env`](https://www.npmjs.com/package/cross-env).

## <a id="toc-making-your-own-preset"></a>Making your own preset

Manually specifying plugins? Plugin options? Environment-based settings? All
this configuration might seem like a ton of repetition for all of your projects.

For this reason, we encourage the community to create their own presets. This
could be a preset for the specific
[node version](https://github.com/leebenson/babel-preset-node5) you are running,
or maybe a preset for your
[entire](https://github.com/cloudflare/babel-preset-cf)
[company](https://github.com/airbnb/babel-preset-airbnb).

It's easy to create a preset. Say you have this `.babelrc` file:

```js
{
  "presets": [
    "es2015",
    "react"
  ],
  "plugins": [
    "transform-flow-strip-types"
  ]
}
```

All you need to do is create a new project following the naming convention
`babel-preset-*` (please be responsible with this namespace), and create two
files.

First, create a new `package.json` file with the necessary `dependencies` for
your preset.

```js
{
  "name": "babel-preset-my-awesome-preset",
  "version": "1.0.0",
  "author": "James Kyle <me@thejameskyle.com>",
  "dependencies": {
    "babel-preset-es2015": "^6.3.13",
    "babel-preset-react": "^6.3.13",
    "babel-plugin-transform-flow-strip-types": "^6.3.15"
  }
}
```

Then create an `index.js` file that exports the contents of your `.babelrc`
file, replacing plugin/preset strings with `require` calls.

```js
module.exports = {
  presets: [
    require("babel-preset-es2015"),
    require("babel-preset-react")
  ],
  plugins: [
    require("babel-plugin-transform-flow-strip-types")
  ]
};
```

Then simply publish this to npm and you can use it like you would any preset.

----

# <a id="toc-babel-and-other-tools"></a>Babel and other tools

Babel is pretty straight forward to setup once you get the hang of it, but it
can be rather difficult navigating how to set it up with other tools. However,
we try to work closely with other projects in order to make the experience as
easy as possible.

## <a id="toc-static-analysis-tools"></a>Static analysis tools

Newer standards bring a lot of new syntax to the language and static analysis
tools are just starting to take advantage of it.

### <a id="toc-linting"></a>Linting

One of the most popular tools for linting is [ESLint](http://eslint.org),
because of this we maintain an official
[`babel-eslint`](https://github.com/babel/babel-eslint) integration.

First install `eslint` and `babel-eslint`.

```sh
$ npm install --save-dev eslint babel-eslint
```

Next create or use the existing `.eslintrc` file in your project and set the
`parser` as `babel-eslint`.

```diff
  {
+   "parser": "babel-eslint",
    "rules": {
      ...
    }
  }
```

Now add a `lint` task to your npm `package.json` scripts:

```diff
  {
    "name": "my-module",
    "scripts": {
+     "lint": "eslint my-files.js"
    },
    "devDependencies": {
      "babel-eslint": "...",
      "eslint": "..."
    }
  }
```

Then just run the task and you will be all setup.

```sh
$ npm run lint
```

For more information consult the
[`babel-eslint`](https://github.com/babel/babel-eslint) or
[`eslint`](http://eslint.org) documentation.

### <a id="toc-code-style"></a>Code Style

> JSCS has merged with ESLint, so checkout Code Styling with ESLint.

JSCS is an extremely popular tool for taking linting a step further into
checking the style of the code itself. A core maintainer of both the Babel and
JSCS projects ([@hzoo](https://github.com/hzoo)) maintains an official
integration with JSCS.

Even better, this integration now lives within JSCS itself under the `--esnext`
option. So integrating Babel is as easy as:

```
$ jscs . --esnext
```

From the cli, or adding the `esnext` option to your `.jscsrc` file.

```diff
  {
    "preset": "airbnb",
+   "esnext": true
  }
```

For more information consult the
[`babel-jscs`](https://github.com/jscs-dev/babel-jscs) or
[`jscs`](http://jscs.info) documentation.

<!--
### Code Coverage

> [WIP]
-->

### <a id="toc-documentation"></a>Documentation

Using Babel, ES2015, and Flow you can infer a lot about your code. Using
[documentation.js](http://documentation.js.org) you can generate detailed API
documentation very easily.

Documentation.js uses Babel behind the scenes to support all of the latest
syntax including Flow annotations in order to declare the types in your code.

## <a id="toc-frameworks"></a>Frameworks

All of the major JavaScript frameworks are now focused on aligning their APIs
around the future of the language. Because of this, there has been a lot of work
going into the tooling.

Frameworks have the opportunity not just to use Babel but to extend it in ways
that improve their users' experience.

### <a id="toc-react"></a>React

React has dramatically changed their API to align with ES2015 classes
([Read about the updated API here](https://babeljs.io/blog/2015/06/07/react-on-es6-plus)).
Even further, React relies on Babel to compile it's JSX syntax, deprecating it's
own custom tooling in favor of Babel. You can start by setting up the
`babel-preset-react` package following the
[instructions above](#babel-preset-react).

The React community took Babel and ran with it. There are now a number of
transforms
[built by the community](https://www.npmjs.com/search?q=babel-plugin+react).

Most notably the
[`babel-plugin-react-transform`](https://github.com/gaearon/babel-plugin-react-transform)
plugin which combined with a number of
[React-specific transforms](https://github.com/gaearon/babel-plugin-react-transform#transforms)
can enable things like *hot module reloading* and other debugging utilities.

<!--
### Ember

> [WIP]
-->

## <a id="toc-text-editors-and-ides"></a>Text Editors and IDEs

Introducing ES2015, JSX, and Flow syntax with Babel can be helpful, but if your
text editor doesn't support it then it can be a really bad experience. For this
reason you will want to setup your text editor or IDE with a Babel plugin.

- [Sublime Text](https://github.com/babel/babel-sublime)
- [Atom](https://atom.io/packages/language-babel)
- [Vim](https://github.com/jbgutierrez/vim-babel)
- [WebStorm](https://babeljs.io/docs/setup/#webstorm)

<!--
# Debugging Babel

> [WIP]
-->

----

# <a id="toc-babel-support"></a>Babel Support

Babel has a very large and quickly growing community, as we grow we want to
ensure that people have all the resources they need to be successful. So we
provide a number of different channels for getting support.

Remember that across all of these communities we enforce a
[Code of Conduct](https://github.com/babel/babel/blob/master/CODE_OF_CONDUCT.md).
If you break the Code of Conduct, action will be taken. So please read it and
be conscious of it when interacting with others.

We are also looking to grow a self-supporting community, for people who stick
around and support others. If you find someone asking a question you know the
answer to, take a few minutes and help them out. Try your best to be kind and
understanding when doing so.

## <a id="toc-babel-forum"></a>Babel Forum

[Discourse](http://www.discourse.org) has provided us with a hosted version of
their forum software for free (and we love them for it!). If forums are your
thing please stop by [discuss.babeljs.io](https://discuss.babeljs.io).

## <a id="toc-babel-chat"></a>Babel Chat

Everyone loves [Slack](https://slack.com). If you're looking for immediate
support from the community then come chat with us at
[slack.babeljs.io](https://slack.babeljs.io).

<!--
## Babel Stack Overflow

> [WIP]
-->

## <a id="toc-babel-issues"></a>Babel Issues

Babel uses the issue tracker provided by
[Github](http://github.com).

You can see all the open and closed issues on
[Github](https://github.com/babel/babel/issues).

If you want to open a new issue:

- [Search for an existing issue](https://github.com/babel/babel/issues)
- [Create a new bug report](https://github.com/babel/babel/issues/new)
  or [request a new feature](https://github.com/babel/babel/issues/new)

### <a id="toc-creating-an-awesome-babel-bug-report"></a>Creating an awesome Babel bug report

Babel issues can sometimes be very difficult to debug remotely, so we need all
the help we can get. Spending a few more minutes crafting a really nice bug
report can help get your problem solved significantly faster.

First, try isolating your problem. It's extremely unlikely that every part of
your setup is contributing to the problem. If your problem is a piece of input
code, try deleting as much code as possible that still causes an issue.

> [WIP]

----

> ***For future updates, follow [@thejameskyle](https://twitter.com/thejameskyle)
> on Twitter.***
