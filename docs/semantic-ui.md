# Semantic UI

  - [Semantic UI](https://semantic-ui.com/)
      - User Interface is the language of the web
      - Semantic is a development framework that helps create beautiful, RESPONSIVE layouts using human-friendly HTML.
      - Unbelievable Theming: Semantic comes equipped with an intuitive INHERITANCE system and high level THEMING VARIABLES that let you have complete design freedom. 有 theme 可以切換，也可以透過 theming variables 覆寫。
      - Responsively Designed: Semantic is designed completely with EM (長度的單位) making responsive sizing a breeze. Design VARIATIONS built into elements allow you to make the choice how content adjusts for tablet and mobile.
      - Partners with Libraries You Love: Semantic has integrations with [React](https://react.semantic-ui.com/), Angular, Meteor, Ember and many other frameworks to help organize your UI layer alongside your application logic. 專注在 UI layer，跟其他 framework 不相衝突；下面 Intuitive Javascript 的範例是用 jQuery

  - [Move over Bootstrap and Foundation, welcome Semantic UI (Example)](https://coderwall.com/p/ham3gg/move-over-bootstrap-and-foundation-welcome-semantic-ui) (2019-01-08) #ril

## 應用實例 {: #powered-by }

  - [Learn Semantic \| Semantic UI](http://learnsemantic.com/) 似乎整個網站都用 Semantic UI 做的。
  - [Semantic\-UI\-Forest, collection of design, themes and templates for Semantic\-UI\.](http://semantic-ui-forest.com/)
  - [Sites/Companies Using Semantic UI · Issue \#2449 · Semantic\-Org/Semantic\-UI](https://github.com/Semantic-Org/Semantic-UI/issues/2449)
  - [23 Best React UI Component Libraries And Frameworks](https://hackernoon.com/23-best-react-ui-component-libraries-and-frameworks-250a81b2ac42#139a) (2018-04-04) Semantic UI React is the official React integration for Semantic UI. With 6.5K stars it’s used by Netflix, Amazon and other great organizations.

## 新手上路 ?? {: #getting-started }

  - 不同的 element 會有不同的 sample，過程中就會學到各式 class 的用法；看過幾個範例，就能體會什麼是 semantic 了。

參考資料：

  - [Semantic UI](https://semantic-ui.com/)
      - Concise HTML: Semantic UI treats WORDS and CLASSES as exchangeable concepts. Classes use syntax from NATURAL LANGUAGES like noun/modifier relationships, word ORDER, and PLURALITY to link concepts intuitively.

        意思是 class 寫起來像是 natural laugnage 一樣 (例如下面的 "three buttons"，注意最後的 `s`)，這就是所謂的 semantic。下面的例子，很神奇的，active button 會自己變色!!

            <div class="ui three buttons">
              <button class="ui active button">One</button> <-- active button 會比 button active 直覺
              <button class="ui button">Two</button>
              <button class="ui button">Three</button>
            </div>

      - Intuitive Javascript: Semantic uses SIMPLE PHRASES called BEHAVIORS that trigger functionality. Any arbitrary decision in a component is included as a setting that developers can modify.

            <select name="skills" multiple="" class="ui fluid dropdown">
              <option value="">Skills</option>
              <option value="angular">Angular</option>
              <option value="css">CSS</option>
              <option value="ember">Ember</option>
              <option value="html">HTML</option>
              <option value="javascript">Javascript</option>
              <option value="meteor">Meteor</option>
              <option value="node">NodeJS</option>
            </select>

            $('select.dropdown') // 執行下去，會讓 list 收合起來，只剩兩個選定的項目，還可以移除
              .dropdown('set selected', ['meteor', 'ember'])
            ;

        其中 `$('select.dropdown')` 是 jQuery 的用法，而 `.dropdown()` 可能是 `semantic.js` 對帶有 `ui` class 的 component 加工後的結果?? 對照 Glossary 的定義，其中 `'set selected'` 就是 behavior -- 可供外面呼叫的 action。

      - Simplified Debugging: Performance logging lets you track down bottlenecks without digging through stack traces. 開啟 Chrome 的 Developer Tools > Console，可以看到一些 debug info，以下面的例子，加上 `debug: true`，就可以看到動畫在每個環節花了多少時間。

            $('.sequenced.images .image')
              .transition({
                debug     : true,
                animation : 'jiggle',
                duration  : 500,
                interval  : 200
              })
            ;

  - [Learn Semantic \| Semantic UI](http://learnsemantic.com/) #ril

## Component, Definition, Element, Collection, View, Module, Behavior ??

  - [Semantic UI](https://semantic-ui.com/)
      - Unbelievable Breadth: DEFINITIONS aren't limited to just buttons on a page. Semantic's COMPONENTS allow several distinct TYPES of definitions: elements, collections, views, modules and behaviors which cover the gamut of interface design.

        整個 UI design 細分為 element、collection、view、module 跟 behavior，泛稱 component，而每一份 component 的說明文件都是一個 definition。UI Docs 左側的選單，除了 Introduction 與 Usage 外，底下依序就是 Globals、Elements、Collections、Views、Modules、Behaviors 不同類型的 component；實務上不太需要管 component type 是什麼，因為有些分類很難理解。

      - Liberate your Development, Enterprise Ready -- Build tools, performance logging, support for CUSTOM DEFINITIONS, multiple-levels of theme inheritance—a developer's dream. 可以自訂元件??

  - [Types of Components - Glossary \| Semantic UI](https://semantic-ui.com/introduction/glossary.html#types-of-components)
      - Globals - Globals are STYLES that are applied ACROSS A SITE. These include things like CSS resets??, and sitewide font, link and sizing defaults. Most importantly, globals include site-wide theming variables that other components can inherit and modify.

        相對於其他 component type，是在頁面上沒有實體的 style，但會影響整個 site 及所有 component type 的呈現。

      - Element - UI elements are page elements with a SINGLE FUNCTION. They can exist ALONE or in a PLURAL FORM with elements sharing QUALITIES (特性). For example, a group of buttons may use `ui red buttons` as a GROUPING with individual `ui button` children.

        實驗確認，兩層都要加 `ui` 才行! (`ui button` 才是個 component，而 `ui red buttons` 只是給了一個 hint 而已) 上面的 plural form 指的是 class name 複數的用法：

            <div class="ui red buttons">
              <a class="ui button">Button 1</a>
              <a class="ui button" href="http://www.google.com">Button 2</a>
            </div>

        只因為 JavaScript + (CSS) 另外分出 module、behavior 的做法有點奇怪? 那是很實作細節的東西，概念上應該都視為 UI element 會比較直覺。

      - Collection - Collections are HETEROGENEOUS groups of components which are usually found together. They describe a list of "usual suspects" (普通嫌疑犯?) which appear in a CERTAIN CONTEXT. They may include and extend other ui elements for use in certain contexts—for example form may extend dropdown or input—as well as include THEIR OWN CONTENT.

        就特定用途 (certain context) 將多個異質的 component 組合起來 (注意組成不限於 element)；最後的 "their own content" 指的是 collection 裡面可能有自己專用的 element (不能拿到外面單獨使用)，例如 form 的 content 裡面有 text area，但不能單獨拿出來用，但也可以有 dropdown，不一定要在 form 裡面使用。

      - Views - A view is a CONVENTION for presenting SPECIFIC TYPES OF CONTENT that is usually CONSISTENT ACROSS A WEBSITE. These include things like comments, activity feeds, or cards.

        只要提供數據、設定即可，不太會去改變它的結構，這樣在外觀上才會形成 convention -- 網路上大家都這麼表現，一看就懂的東西。

      - Modules - Modules are components that include both a definition of how they appear and how they BEHAVE. These include components like, accordions, dropdowns, and popups. 也就是 CSS + JavaScript。
      - Behaviors - Behaviors are standalone Javascript components that describe how page elements should ACT, but not how they should APPEAR. Behaviors include things like form validation, state management, and API request routing. 相對於 module 就少了 CSS 而已。

  - [Project Terminology - Glossary \| Semantic UI](https://semantic-ui.com/introduction/glossary.html#project-terminology)
      - Component - A component is a GENERAL TERM used to refer to any user interface element packaged for distribution.
      - Definition - A definition is a set of CSS and Javascript which describe a component's essential qualities.

        Definition use variables to express arbitrary parts of a component's appearance so that they can be changed with themes.

      - `ui` is a special class name used to distinguish parts of components from components. 只用在 component 的層級，例如 list element 用 `ui list`，但裡面的 item 只會用 `item` (而非 `ui item`)。

        The ui class name helps encapsulate CSS rules by making sure all 'parts of a component' are defined in context to a 'WHOLE' component. 大概就是這樣的感覺，不過像 [card](https://semantic-ui.com/views/card.html) 這類較複雜的 component，巢狀的 `ui` 是可能出現的。

        It also helps make SCANNING unknown code simpler. If you see `ui` you know you are looking at a component. 快速識別出 Semantic UI 作用的範圍

  - [Definition Terminology - Glossary \| Semantic UI](https://semantic-ui.com/introduction/glossary.html#definition-terminology) #ril
      - When browsing Semantic UI definitions, you will see content grouped into different sections. These parts of a definition are consistent across definitions, and are common patterns for describing components.

        原來 UI Docs 中除 Introduction 與 Usage，每份文件都劃分成 Types、Variations 等不同的段落，而每份文件也各自構成一個 definition。

      - Content - Content are parts which ONLY HAVE MEANING IN THE CONTEXT OF A COMPONENT. Content use NAMES (指 class name) which describe the type of expected content like header, description, menu, or item.

        Content inside a collection or view often includes STUBBED (簡化的?) versions of other components. For example cards let you use image content, which can be EXTENDED by using ui image variations. 若可以擴充，可以解釋成 collection 與 view 的文件在說明 content 時，只會簡單說明，完整的用法要看個別 component 文件。

      - Behaviors - Behaviors are ACTIONS that a component can perform. Behaviors are represented by simple phrases like "set value" or "increment", that can be invoked in Javascript. Behaviors are AUTOMATICALLY CONVERTED from spaced lowercase phrases to internal Javascript methods at invocation.

        以首頁提到的 `$('select.dropdown').dropdown('set selected', ['meteor', 'ember'])` 為例，其中 `dropdown()` 的用法可以在 [definition 裡的 Usage > Behavior](https://semantic-ui.com/modules/dropdown.html#behavior) 查到 -- `dropdown('behavior name', argumentOne, argumentTwo)`，而 `set selected(value)` 用來 Sets value as selected。

## Message ??

  - [Message \| Semantic UI](https://semantic-ui.com/collections/message.html) #ril
      - A message displays information that explains NEARBY CONTENT；表現上比較像是 message box，因為有個框框，左邊可以有 icon。
      - Type 分為 (basic) message、list message、icon message 與 dismissable block (搭配 JavaScript)；其實 types 間也不完全互斥，而是可以混搭的元素，裡面可以有 list、icon、右上角的叉叉 (可以關閉 message)。
      - Message box 主要用 `ui message` 識別，裡面的標題用 `header` 識別，如果有清單用 `<ul class="list">` 即可，下面的 `<li>` 不用特別宣告 class：

            <div class="ui message">
              <div class="header">
                New Site Features
              </div>
              <ul class="list">
                <li>You can now have cover images on blog pages</li>
                <li>Drafts will now auto-save while writing</li>
              </ul>
            </div>

      - Possive/success 或 negative/error message，加上 `positive`/`success` 或 `negatice`/`error` 即可，例如 `<div class="ui negative message">`。
      - Icon 會出大大地出現在 message box，不過這要在最外層加上 `icon` 識別，裡面也要用 `content` 內容區隔開來 (icon 在左側，內容在右側)，例如：

            <div class="ui icon message">
              <i class="inbox icon"></i>
              <div class="content">
                <div class="header">
                  Have you heard about our mailing list?
                </div>
                <p>Get the best news in your e-mail every day.</p>
              </div>
            </div>

## Theme ??

  - [Types of Components - Glossary \| Semantic UI](https://semantic-ui.com/introduction/glossary.html#types-of-components) Most importantly, globals include site-wide theming variables that other components can inherit and modify.
  - [Theming \| Semantic UI](https://semantic-ui.com/usage/theming.html) #ril
  - [Message \| Semantic UI](https://semantic-ui.com/collections/message.html) 只有 3 種 theme? 跟 button 的 11 種 theme 差很多，會不會有不一致的問題??

## Image ??

  - [Image \| Semantic UI](https://semantic-ui.com/elements/image.html) #ril
      - An image is a GRAPHIC REPRESENTATION of something 這說法滿特別的
      - Unless a size is specified, images will use the ORIGINAL DIMENSIONS of the image UP TO the size of its container.. 所以大圖放進小的 container 會被限制住??
      - You can specify an `img` or `svg` as a `ui image` or use a child element.

            <img class="ui small image" src="/images/wireframe/image.png">

## Icon

  - [Icons - Icon \| Semantic UI](https://semantic-ui.com/elements/icon.html)
      - An icon is a glyph used to represent something else. An icon set contains an arbitrary number of glyphs
      - Icons serve a very similar function to text in a page. In Semantic icons receive a special tag `<i>` which allow for an ABBREVIATED MARKUP when sitting along-side text. 例如：

            <i class="circle icon"></i>

        為了讓 icon 與文字混用時的 markup 可以簡短一點就用 `<i>`，聽起來怪怪的? 不過背後用字型來表現，好像也就不奇怪了。

      - Semantic includes a complete PORT of Font Awesome 5.0.8 designed by the FontAwesome team for its standard icon set.

## Checkbox ??

  - [Checkbox \| Semantic UI](https://semantic-ui.com/modules/checkbox.html) #ril
      - A checkbox allows a user to select a value from a SMALL SET OF OPTIONS, often binary 更多選項，Dropdown > Multiple Selection 會更合適。
      - Checkbox - A standard checkbox

            <div class="ui checkbox">
              <input type="checkbox" name="example">
              <label>Make my profile visible</label>
            </div>

## Menu ??

  - [Menu \| Semantic UI](https://semantic-ui.com/collections/menu.html) #ril
      - A menu displays grouped NAVIGATION actions
      - Starting in 2.0 menus now use flexbox. This allows each menu item to automatically stretch to the size of the largest item. 最簡單的 menu 看起來像 (group of) buttons。

## 安裝設定 {: #installation }

```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.css">
<script src="https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.js"></script>
```

---

參考資料：

  - [Preface - Getting Started \| Semantic UI](https://semantic-ui.com/introduction/getting-started.html#preface) #ril
      - Semantic UI packaged Gulp build tools so your project can preserve its own theme changes. The easiest way to install Semantic UI is our NPM package which contains special install scripts to make setup interactive and updates seamless.
      - For integrating Semantic UI tasks into your own build tools, or using a CDN see our recipes section.

  - [CDN Releases - Recipes \| Semantic UI](https://semantic-ui.com/introduction/advanced-usage.html#cdn-releases) Individual components are available on [jsDelivr](https://www.jsdelivr.com/projects/semantic-ui):

        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.css">
        <script src="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.js"></script>

    jQuery 要另外引用 (且一定要放 `semantic.min.js` 前面)，否則會出現 `Uncaught ReferenceError: jQuery is not defined at semantic.min.js:11` 的錯誤。

  - [Include in Your HTML - Getting Started \| Semantic UI](https://semantic-ui.com/introduction/getting-started.html#include-in-your-html) #ril
      - Semantic UI 自己的 CSS 及 JavaScript，但也會用到 jQuery。

            <link rel="stylesheet" type="text/css" href="semantic/dist/semantic.min.css">
            <script
              src="https://code.jquery.com/jquery-3.1.1.min.js"
              integrity="sha256-hVVnYaiADRTO2PzUGmuLJr8BLUSjGIZsDYGmIJLv2b8="
              crossorigin="anonymous"></script>
            <script src="semantic/dist/semantic.min.js"></script>

        其中 `crossorigin="anonymous"` 的作用是什麼??

## 參考資料 {: #reference }

  - [Semantic UI](https://semantic-ui.com/)
  - [Semantic-Org/Semantic-UI - GitHub](https://github.com/semantic-org/semantic-ui/)

社群：

  - ['semantic-ui' Questions - Stack Overflow](https://stackoverflow.com/questions/tagged/semantic-ui)

更多：

  - [Form](semantic-ui-form.md)
  - [Layout](semantic-ui-layout.md)

相關：

  - [Responsive Web Design (RWD)](rwd.md)

手冊：

  - [All UI](https://semantic-ui.com/kitchen-sink.html)
  - [Icon Sets](https://semantic-ui.com/elements/icon.html)
