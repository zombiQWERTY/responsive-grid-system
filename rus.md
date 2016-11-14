<article>
Одним из лучших дополнений для нестандартного web-дизайна является 
специально сделанная для него отзывчивая сетка. Вы можете настроить 
все, что вам нужно, включая количество колонок, размер колонок и отступов, 
и даже контрольные точки, в которых вы перестраиваете вашу раскладку.

К сожалению, многие люди даже не пытаются делать сетки для своих
web-сайтов, т.к. у них не хватает знаний и уверенности, что бы попробовать.

Итак, в этой статье, я хочу помочь вам зарядиться знаниями и верой в себя, которые 
вам понадобятся для создания пользовательской сетки. Надеюсь, что к концу этой статьи,
вы сможете оторваться от фреймворков и испытаете пользовательскую сетку на
вашем следующем проекте.

## Что входит в сетку? {#what-goes-into-a-grid-system}

Вам нужно знать три вещи, перед тем как делать вашу сетку

**Во-первых, вам нужно спроектировать вашу сетку**.

Вы используюете колонки одинаковой или разной ширины? Как много колонок у вас
есть? Какие размеры у ваших отступов и колонок?

Вы сможете правильно расчитать сетку только когда у вас будут ответы на вопросы выше.
Что бы помочь вам, я написал статью о [проектировании сеток][1]. Прочтите ее, если вы
хотите знать как спроектировать сетку.

**Во-вторых, вам нужно знать как ваша сетка ведет себе на разных вьюпортах.**

Будете ли вы менять размеры колонок и отступов пропорционально, когда меняется ширина
вьюпорта? Будете ли вы изменять колонки, при этом фиксируя отступы? Будете ли вы менять 
количество колонок в конкретных контрольных точках?

Вам также нужно ответить на эти вопросы. На базе этого вы поймете, как расчитать
ширину ваших колонок и отступов. Я также написал свои соображения об этом в [этой статье][2],
так что прочтите ее, если вы сомневаетесь.

**В-третьих, вы хотите классы сетки в вашем HTML?**

Когда дело доходит до (типографских) сеток, мир фронтенда делится на две фракции.

Одна пишет классы сетки в HTML (так делают, например,  Bootstrap и Foundation).
Я называю это **HTML-сетки**. Их HTML выглядит так: 

    <div class="container">
      <div class="row">
        <div class="col-md-9">Content</div>
        <div class="col-md-3">Sidebar</div>
      </div>
    </div>
    
Другая фракци создает сетки на CSS. Я называю это **CSS-сетки**.

*HTML для CSS-сеток проще* по сравнению с HTML для *HTML-сеток.*
Вам нужно меньше разметки для того, что создаете. Вам также не нужно
помнить какие у сетки классы:

    <div class="content-sidebar">
      <div class="content"></div>
      <div class="sidebar"></div>
    </div>
    
С другой стороны, *CSS для CSS-сеток более сложен*. 
Нужно хорошенько подумать даже для реализации простой задачи 
(если раньше вы таких сеток не создавали).
 
**Что бы я выбрал?**

Многие фронтенд-эксперты выбирают CSS-сетки. Я тоже во фракции CSS-сеток
(несмотря на то, что я не смею называть себя экспертом).

Я написал о том, почему я выбрал CSS-сетки заместо HTML-сеток
в [другой статье][3], если вам интересно это выяснить. Еще я написал [статью][4],
которая поможет вам перейти с HTML-сеток на CSS-сетки, если вы 
хотите переключиться.

(Так много статей читать... :cry:)

В любом случае, это те три вещи, которые вам нужно знать перед тем как 
вы сможете сделать вашу сетку. В кратце, вот они: 

1.  Дизайн вашей сетки
2.  Как ваша сетка ведет себя на разных вьюпортах
3.  Следует ли использовать CSS-сетки или HTML-сетки

Мы двинемся дальше, только если у нас есть эти компоненты. Вот то, что мы будем
использовать далее в статье:

1.  Сетка имеет максимальную *ширину в 1140px*, с *12 колоноками по 75px* и 
    *отступами в 20px*. (Прочтите [эту статью][2] с советами о том, 
    как получить эти числа)
2.  Когда вьюпорт меняет размеры, колонки должны менять размеры пропорционально,
    в то время как *отступы остаются фиксированными* с шириной 20px. 
    (Прочтие [эту статью][2], что бы понять, почему я выбрал такое поведение).
3.  Я собираюсь использовать *CSS-сетки*. (Прочтите [эту статью][3], 
    что бы понять, почему я рекомендую их).

С этими словами, давайте начнем!

## Создаем вашу сетку {#building-your-grid-system}

Есть восемь шагов для того, что сделать вашу сетку. Вкратце, вот эти шаги: 

1.  Выберите спецификацию для сетки
2.  Установите `box-sizing` в `border-box` 
3.  Создайте контейнер для сетки
4.  Расчитайте ширину колонок
5.  Определите расположение отступов
6.  Создайте сетку для отладки
7.  Внесите изменения в разметку
8.  Сделайте вашу разметку адаптивной

Большинство из этих шагов становятся достаточно простыми, как только
вы сделаете их хотя бы раз. Я обстоятельно объясню все, что вам нужно знать, пока
мы делаем каждый шаг.

## Шаг 1: Выберите спецификацию {#step-1-choose-a-spec}

Вы используете в вашей сетке *CSS Grid*, *Flexbox*, или старые добрые *флоаты*?
Ваши рассуждения и детали реализации будут разными для каждой спецификации.

Из всех трех спецификаций, CSS Grid на сегодняшний день является лучшим инструментов 
для создания сетки (из-за сетки :sunglasses:). К сожалению, поддержка CSS Grid 
оставляет желать лучшего, и мы не можем использовать его прямо сейчас. 
Каждый браузер прячет CSS Grid Layout под флаг, и это значит,
что мы не будет рассматривать его в статье. Я настоятельно рекомендую посмотреть
[работу Рейчел Эндрю (Rachel Andrew)][5], если вам интересен CSS Grid.

Далее, перейедм к Flexbox и флоатам. Рассуждения при использовании этих двух
спецификаций похожи, так что вы можете выбрать любую из них и читать статью дальше.
Я буду использовать флоаты, потому что их проще объяснить и они лучше
подходят для новичков.

Если вы выбрали Flexbox, имеейте ввиду, что есть несколько нюансов, которые вам 
нужно учесть.

## Шаг 2: Установите box-sizing в border-box {#step-2-set-box-sizing-to-border-box}

Свойство `box-sizing` меняет дефолтные настройки блочной CSS модели, которая
используется браузерами для расчета свойств `width` и `height`. Выставляя `box-sizing`
в `border-box`, мы сильно упрощаем расчет размеров колонок и отступов
(вы увидите это позже).

Вот картинка, которое показывает как `width` вычисляется в зависимости от значений
`box-sizing`<figure>

![Свойство Box-sizing и как оно влияет на расчет ширины][6]<figcaption>
Свойство Box-sizing и как оно влияет на расчет ширины</figcaption></figure>

Обычно я ставлю `box-sizing` в `border-box` для всех элементов на сайте, 
так что расчеты `width` и `height` остаются последовательными (и интуитивно понятными)
по всем направлениям. Вот как я делаю это:

    html {
      box-sizing: border-box;
    }
    
    *,
    *:before,
    *:after {
      box-sizing: inherit;
    }
    
Примечания: если вам нужно более детальное объяснение `box-sizing`, я рекомендую
вам [прочесть эту статью][7].


## Шаг 3: Создайте контейнер для сетки {#step-3-create-the-grid-container}

У каждой сетки есть контейнер, который определяет её максимальную ширину.
Как правило, я называю его `.l-wrap`. Префикс `.l-` означает layout, и я использую 
это соглашение по именованию с тех пор, как изучил [SMACSS][8] от 
[Джонатана Снука (Jonathan Snook)][9].

    .l-wrap {
        max-width: 1140px;
        margin-right: auto;
        margin-left: auto;
    }

Примечание: я настоятельно рекомендую использовать относительные единицы типа
`em` или `rem` заместо пикселов для доступности и отзывчивости. В этой стаье
я пишу все в пикселах, потому что они проще для понимания.


## Шаг 4: Расчитайте ширину колонок {#step-4-calculate-column-width}

Помните, мы используем флоаты для создания наших колонок и отступов. Когда мы
используем флоаты, у нас есть только пять свойств для этого 
(если вы используете Flexbox, то у вас есть еще несколько); вот эти
пять свойств:

*   width
*   margin-right
*   margin-left
*   padding-right
*   padding-left

Если вы помните, HTML для CSS-сеток выглядит примерно так:
    
    <div class="l-wrap">
      <div class="three-col-grid">
        <div class="grid-item">Grid item</div>
        <div class="grid-item">Grid item</div>
        <div class="grid-item">Grid item</div>
      </div>
    </div>
    

Мы знаем, что в этом HTML сетка имеет всего три колонки в строке. Мы также
знаем, что тут нет дополнительно `<div>` для создания отступов. Это означает:

1.  Мы создаем колонки со свойством `width`
2.  Мы создаем отступы либо со свойством `margin`, либо со свойством `padding`

Если думать о колонках и отступах одновременно, то становится сложнее,
так что давайте допустим, что сперва мы создаем сетку без отступов.

На выходе такая сетка будет похожа на что-то такое:<figure>

![Трех-колоночная сетка без отступов][10]<figcaption>Трех-колоночная 
сетка без отступов</figcaption></figure>

Это тот момент, в который мы должны сделать немного математических вычислений.
Мы знаем, что сетка имеет максимальную ширину в 1140px, а это означает, что
каждая колонка будет 380px (`1140 ÷ 3`).

    .three-col-grid .grid-item {
      width: 380px;
      float: left;
    }
    

Пока все хорошо. Мы сделали сетку, которая работает отлично на вьюпортах больше
чем 1140px. К сожалению, все ломается, когда вьюпорт меньше, чем 1140px.<figure>

![Сетка ломается меньше 1140px][11]<figcaption>Сетка ломается меньше 
1140px</figcaption></figure>

Это занчит, что мы не можем использовать пикселы в наших расчетах. Нам нужна 
единица, которая перестроится в соответствии с шириной контейнера. Лишь одна
единица может так делать - проценты (`%`). Поэтому, мы пропишем ширину в 
процентах:

    .three-col-grid .grid-item  {
      width: 33.33333%;
      float: left;
    }
    
Полученные код выше  - это простая трех-колоночная сетка без каких-либо отступов. 
Когда браузер меняет размер, эти три колонки сменят размер пропорционально.<figure>

![Три колонки без отступов][12]<figcaption>Три колонки без 
отступов</figcaption></figure>

Еще кое-что, перед тем как мы двинемся дальше. Всякий раз, когда все дочерние
элементы имеют обтекание в контейнере, высота контейнера схлопывается. Этот 
феномен называется [схлопывание флоата][13]. Это как если бы в 
контейнере не было бы никаких дочерних элементов:<figure>

![Схлопывание флоата. Изображение с CSS Tricks][14]<figcaption>Схлопывание 
флоата (изображение с CSS Tricks)</figcaption></figure>

Для того, что бы это исправить, нам нужен clearfix, который выглядит следующим
образом:

    .three-col-grid:after {
      display: table;
      clear: both;
      content: '';
    }
    

Если вы используете препроцессор типа Sass, вы можете сделать из этого 
миксину, что позволит вам использовать этот код в разных местах:

    
    // Clearfix
    @mixin clearfix {
      &:after {
        display: table;
        clear: both;
        content: '';
      }
    }
    
    // Usage
    .three-col-grid { @include clearfix; }
    
После того как мы закончили с колонками, следующим шагом создадим несколько 
отступов.

## Шаг 5: Определите расположение отступов {#step-5-determine-gutter-position}

So far, we know we should create gutters either with `margin` or `padding`
properties. But which should we use?

If you sketch around for a bit, you’ll quickly notice that you have four
possible ways to create these gutters.

1.  Gutters can be placed on *one side*, as *margins* 
2.  Gutters can be placed on *one side*, as *paddings* 
3.  Gutters can be split equally on *both sides*, as *margins* 
4.  Gutters can be split equally on *both sides*, as *paddings* <figure>

![4 possible ways to create columns and gutters][15]<figcaption>4 possible ways
to create columns and gutters</figcaption></figure>
This is where it starts to get complicated. You need to calculate column widths
differently depending on which method you use.

We’ll go through these methods one by one and look at the differences. Take
your time as you read through them.

Here we go:

With this method, you create gutters with the `margin` property. This gutter
will either be placed on the left or right of the columns; it’s up to you which 
side to choose.

For the purpose of this article, let’s say you chose to put your gutters on
the right. What you’ll do then is:

    <p class="hljs-selector-class">.grid-item</p> {
      ;
      <p class="hljs-attribute">margin-right</p>: <p class="hljs-number">20px</p>;
      <p class="hljs-attribute">float</p>: left;
    }
    

Then, you recalculate your column-width according to this image:<figure>

![One-sided gutters using margins][16]<figcaption>One-sided gutters using
margins</figcaption></figure>
You can see from the image above that *1140px* is equal to *three columns* and*
two gutters*.

And we have a problem here… We need columns to be written in percentages, but
our gutters are fixed at 20px. We can’t do math with two different units at once!

Well, it wasn’t possible before, but it is now.

You can use the CSS `calc` function to mix percentages with other units. It
retrieves the unit values of the percentages to perform calculations on the fly.

What this means is you can leave your width as a function, and browsers will
automatically calculate your values for you:

    <p class="hljs-selector-class">.grid-item</p> {
      <p class="hljs-attribute">width</p>: <p class="hljs-built_in">calc</p>((100% - 20px * 2) / );
      
    }
    

That’s great.

After getting the column width, you need to remove the final gutter from the
rightmost grid item. Here’s how you can do it:

    <p class="hljs-selector-class">.grid-item</p>
    <p class="hljs-selector-pseudo">:last-child</p> {
      <p class="hljs-attribute">margin-right</p>: ;
    }
    

Most of the time, when you remove the final gutter on the rightmost item, you
also want to float it to the right to prevent subpixel rounding errors from 
messing up your grid by sending the last item into the next row. This only 
happens on browsers that round subpixels up.<
e>


![Subpixel rounding errors might break the grid by pushing the final item to the next row][17]
    <p class="hljs-selector-class">.grid-item</p>
    <p class="hljs-selector-pseudo">:last-child</p> {
      <p class="hljs-attribute">margin-right</p>: ;
      <p class="hljs-attribute">float</p>: right;
    }
    

Phew. Almost there. Just one more thing.

The code so far is great if our grid contains only a single row. It doesn’t
cut it, however, if there’s more than one row of items
😢.<figure>

![Our code fails if there's more than one row][18]<figcaption>Our code fails if
there’s more than one row</figcaption></figure>
What we need to do is to remove the right margin from the rightmost item in
every row. The best way to do this is with`nth-child()`:

    
    <p class="hljs-selector-class">.grid-item</p>
    <p class="hljs-selector-pseudo">:nth-child(3n+3)</p> {
      <p class="hljs-attribute">margin-right</p>: ;
      <p class="hljs-attribute">float</p>: right;
    }
    

That’s all you need for a one-sided gutter built with margins. Here’s a
codepen for you to play around with.

See the Pen [Single sided grid with gutters as margins][19] by Zell Liew (
[@zellwk][20]) on [CodePen][21].

Note: Calc doesn’t work with IE8 and Opera mini. You might want to consider
other methods if you need to support these two browsers.

Like the one-sided gutters with margins, this method requires you to place your
gutters to one side of your columns as well. Let’s say you choose the right side
again.

    <p class="hljs-selector-class">.grid-item</p> {
      
      <p class="hljs-attribute">padding-right</p>: <p class="hljs-number">20px</p>;
      <p class="hljs-attribute">float</p>: left;
    }
    

Then, you can recalculate your column-width according to this image:<
e>

![One-sided gutters using padding][22]<figcaption>One-sided gutters using
padding</figcaption></figure>
Notice the widths are different from the previous method? They’re different
because we switched the`box-sizing` property to `border-box`. Now, `width`
calculations include`padding` in them.

In this case, two of the three columns have a larger width than the final one,
which eventually results in weird calculations and CSS code that’s hard to grasp.

I suggest not even attempting this method. (It’s going to be really ugly if
you continue with it. Try it at your own risk!
)

## Method 3: Split gutters (Margin) {#method-3-split-gutters-margin-}

In this method, you split gutters into two and place each half on the sides of
your columns. The code looks like this:

    <p class="hljs-selector-class">.grid-item</p> {
      
      <p class="hljs-attribute">margin-right</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">margin-left</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">float</p>: left;
    }
    

Then, you calculate your column-width according to this image:<figure>

![Split gutters with margin][23]<figcaption>Split gutters with margin</
figcaption
></figure>
From what we know before, you need to calculate the column-width with the 
`calc()` function. In this case, you remove three gutters from 100% before
dividing the answer by three to get your column-width. In other words, the 
column-width is`calc((100% - 20px * 3) / 3)`.

    <p class="hljs-selector-class">.grid-item</p> {
      <p class="hljs-attribute">width</p>: <p class="hljs-built_in">calc</p>((100% - 20px * 3) / );
      <p class="hljs-attribute">margin-right</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">margin-left</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">float</p>: left;
    }
    

That’s it! (Nothing extra you need to do for grids with multiple rows 😉).
Here’s a codepen for you to play with:

See the Pen [grid with split gutters as margins][24] by Zell Liew (
[@zellwk][20]) on [CodePen][21].

## Method 4: Split gutters (Padding) {#method-4-split-gutters-padding-}

This method is similar to the previous one. You split your gutters and place
each half on the sides of your columns as well. This time, you use padding 
instead of gutters.

    <p class="hljs-selector-class">.grid-item</p> {
      
      <p class="hljs-attribute">padding-right</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">padding-left</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">float</p>: left;
    }
    

Then, you calculate your column widths as follows:<figure>

![Split gutters with padding][25]<figcaption>Split gutters with padding</
figcaption
></figure>
Notice the column-widths are much easier to calculate this time? That’s right
; it’s a third of the grid width at every breakpoint.

    <p class="hljs-selector-class">.grid-item</p> {
      <p class="hljs-attribute">width</p>: <p class="hljs-number">33.3333%</p>;
      <p class="hljs-attribute">padding-right</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">padding-left</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">float</p>: left;
    }
    

Here’s a codepen for you to play with:

See the Pen [grid with split gutters as padding][26] by Zell Liew (
[@zellwk][20]) on [CodePen][21].

Before we move on, I want to tell you about a small caveat if you use split
gutter with padding. If you take a look at the markup in the Codepen, you’ll 
notice that I added an extra`<div>` within `.grid-item`. This extra 
`<div>` is required if your component contains background or borders.

This is because background is shown on padding properties. This image should
explain why (hopefully), by showing the relationship between`background` and
other properties.<figure>

![Background is shown on the padding property][27]<figcaption>Background is
shown on the padding property</figcaption></figure>
### What would I use? {#what-would-i-use-}

When I started to code grids about two years ago, I mostly coded grids that are
designed with the[top-down approach][28] and built with a [hybrid system][29].
In that approach/system,*I used percentages for both width and gutter values*

At that time, I loved the simplicity of setting gutters on one side of the grid
. There was less cognitive overload for me because I’m pretty bad with math. The
extra`gutters ÷ 2` calculation turned me off quickly.

I’m thankful I went that route. Although the CSS seems more complicated than
split gutters, I was forced to learn[nth-child properly][30]. I also learned
the importance of writing[mobile-first CSS][31], both which are still major
impediments to both young and experienced developers, as far as I can tell.

However, if you ask me to choose now, **I’ll go for split gutters** instead of
single-sided ones, because the CSS is so much simpler. Also,**I prefer using
margin for gutters** instead of padding because of the cleaner markup. (But *
padding is easier to calculate*, so I’ll continue the rest of the article
with padding
).

## Step 6: Create a debug grid {#step-6-create-a-debug-grid}

When you’re starting out, it’s especially helpful to have a control grid
around to help you debug your layouts. It helps ensure you’re building things 
correctly.

At this point, I only know a lame way to create the debug grid. That is to
create HTML elements, and add some CSS to it. Here’s what the HTML looks like:

    <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"fixed-gutter-grid"</p>>
      <p class="xml"></p>
    <p class="hljs-tag"><</p>
    <p class="hljs-name">div</p> <p class="hljs-attr">class</p>=<p class="hljs-string">"column"</p>><p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"column"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
    <<p class="hljs-regexp">/div></p>
    

The CSS for this debug grid is the following (I’m using split gutters with
margins to reduce markup for the debug grid
):

    <p class="hljs-selector-class">.column</p> {
      <p class="hljs-attribute">width</p>: <p class="hljs-built_in">calc</p>((100% - 20px * 12) / );
      <p class="hljs-attribute">height</p>: <p class="hljs-number">80px</p>;
      <p class="hljs-attribute">margin-right</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">margin-left</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">background</p>: <p class="hljs-built_in">rgba</p>(0, 0, 255, 0.25);
      <p class="hljs-attribute">float</p>: left;
    }
    

See the Pen [Fixed gutter debug grid][32] by Zell Liew ([@zellwk][20]) on 
[CodePen][21].

(Ultra side note: Miriam and Robson are working on a 
[SVG-background image debug grid on Susy v3][33]. This is super exciting cause
you can use a simple function to create your debug grid!
)

## Step 7: Create layout variations {#step-7-create-layout-variations}

The next step is to create your layout variations based on your content. This
is where CSS grid systems shine. Instead of creating layouts by writing multiple
grid classes, you can create a reasonable-sounding name for your layout.

For instance, let’s say you have this grid layout that’s only used for
guest articles. The layout looks like this on desktop:<figure>

![Example grid layout that's only used for guest articles][34]<figcaption>
Example grid layout that’s only used for guest articles</figcaption></figure>
The markup for this guest-article layout can be:

    <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"l-guest-article"</p>>
      <p class="xml"></p>
    <p class="hljs-tag"><</p>
    <p class="hljs-name">div</p> <p class="hljs-attr">class</p>=<p class="hljs-string">"l-guest"</p>> <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"l-main"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
      <div <p class="hljs-class"></p>
    <p class="hljs-keyword">class</p>=<p class="hljs-string">"l-sidebar"</p>><p class="xml"></p>
    <p class="hljs-tag"></</p>
    <p class="hljs-name">div</p>>
    <<p class="hljs-regexp">/div></p>
    

Alright sweet. So we have 12 columns now. The width of one column is 8.333% 
`(100 ÷ 12)`.

The width of `.l-guest` is two columns. So, what you do is multiple 8.333% by
two. Simple as that. Just rinse and repeat for the rest.

Here, I suggest using a preprocessor like Sass, which allows you to calculate
column width easily with a`percentage` function instead of doing the
calculations manually:

    <p class="hljs-selector-class">.l-guest-article</p> {
      @<p class="hljs-keyword">include</p> clearfix;
      <p class="hljs-selector-class">.l-guest</p> {
        
        <p class="hljs-attribute">width</p>: percentage(/);
        <p class="hljs-attribute">padding-left</p>: <p class="hljs-number">10px</p>;
        <p class="hljs-attribute">padding-right</p>: <p class="hljs-number">10px</p>;
        <p class="hljs-attribute">float</p>: left;
      }
    
      <p class="hljs-selector-class">.l-main</p> {
        <p class="hljs-attribute">width</p>: percentage(/);
        <p class="hljs-attribute">padding-right</p>: <p class="hljs-number">10px</p>;
        <p class="hljs-attribute">padding-left</p>: <p class="hljs-number">10px</p>;
        <p class="hljs-attribute">float</p>: left;
      }
    
      <p class="hljs-selector-class">.l-sidebar</p> {
        <p class="hljs-attribute">width</p>: percentage(/);
        <p class="hljs-attribute">padding-right</p>: <p class="hljs-number">10px</p>;
        <p class="hljs-attribute">padding-left</p>: <p class="hljs-number">10px</p>;
        <p class="hljs-attribute">float</p>: left;
      }
    }
    

See the Pen [Content-sidebar-layout with fixed-gutter grid][35] by Zell Liew
([@zellwk][20]) on [CodePen][21].

You probably find that there’s a lot of code repetition about now. We can
make it nicer by abstracting the common parts away into a separate selector like
`.grid-item`.

    <p class="hljs-selector-class">.grid-item</p> {
      <p class="hljs-attribute">padding-left</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">padding-right</p>: <p class="hljs-number">10px</p>;
      <p class="hljs-attribute">float</p>: left;
    }
    
    <p class="hljs-selector-class">.l-guest-article</p> {
      <p class="hljs-selector-class">.l-guest</p> { <p class="hljs-attribute">width</p>: percentage(/);}
      <p class="hljs-selector-class">.l-main</p> { <p class="hljs-attribute">width</p>: percentage(/);}
      <p class="hljs-selector-class">.l-sidebar</p> { <p class="hljs-attribute">width</p>: percentage(/); }
    }
    

There. Much cleaner. :)

## Step 8: Make your layouts responsive {#step-8-make-your-layouts-responsive
}

The final step is to make your layouts responsive. Let’s say our guest
article layout responds in the following way:<figure>

![How guest the guest article layout respond to different viewports][36]<
figcaption>How guest the guest article layout respond to different viewports</
figcaption
></figure>
The markup of our guest article shouldn’t change. What we have is the most
accessible layout we can possible have. So, the changes should entirely be in 
CSS.

When writing the CSS for our responsive guest layout, I highly recommend you
write[mobile first css][37] because it makes your code simpler and neater. We
can begin by writing CSS for the mobile layout first.

Here’s the code:

    <p class="hljs-selector-class">.l-guest-article</p> {
      <p class="hljs-selector-class">.l-guest</p> {  }
      <p class="hljs-selector-class">.l-main</p> {
        <p class="hljs-attribute">margin-top</p>: <p class="hljs-number">20px</p>;
      }
      <p class="hljs-selector-class">.l-sidebar</p> {
        <p class="hljs-attribute">margin-top</p>: <p class="hljs-number">20px</p>;
      }
    }
    

There’s nothing we need to do since every component takes up the full width
by default. However, we can add some margin-top to the last two items to 
separate the elements from each other.

Next, let’s move on to the tablet layout.

For this layout, let’s say we activate the breakpoint is 700px. `.l-guest`
should be 4 of 12 columns while`.l-main` and `.l-sidebar` should be 8 of 12
columns each.

Here, we need to remove the `margin-top` property from `.l-main` because it
needs to be in line with`.l-guest`.

Also, if we set `.l-sidebar` to a width of 8 columns, it will automatically
float onto the second row because there’s not enough room on the first row. 
Since it’s on the second row, we also need to add some left margins on
`.l-sidebar` to push it into position; alternatively, we can float it to the
right. (I’ll float right since there’s no need to calculate anything
).

Finally, since we’re floating the grid items, the grid container should
include a clearfix to clear it’s own children.

    <p class="hljs-selector-class">.l-guest-article</p> {
      @<p class="hljs-keyword">include</p> clearfix;
      <p class="hljs-selector-class">.l-guest</p> {
        @<p class="hljs-keyword">media</p> (min-width: 700px) {
          <p class="hljs-attribute">width</p>: percentage(/);
          <p class="hljs-attribute">float</p>: left;
        }
      }
      <p class="hljs-selector-class">.l-main</p> {
        <p class="hljs-attribute">margin-top</p>: <p class="hljs-number">20px</p>;
        @<p class="hljs-keyword">media</p> (min-width: 700px) {
          <p class="hljs-attribute">width</p>: percentage(/);
          <p class="hljs-attribute">margin-top</p>: ;
          <p class="hljs-attribute">float</p>: left;
        }
      }
      <p class="hljs-selector-class">.l-sidebar</p> {
        <p class="hljs-attribute">margin-top</p>: <p class="hljs-number">20px</p>;
        @<p class="hljs-keyword">media</p> (min-width: 700px) {
          <p class="hljs-attribute">width</p>: percentage(/);
          <p class="hljs-attribute">float</p>: right;
        }
      }
    }
    

Lastly, let’s move on to the desktop layout.

For this layout, let’s say we activate the breakpoint is 1200px. `.l-guest`
should be 2 of 12 columns,`.l-main` should be 7 of 12 columns and `.l-sidebar`
should be 3 of 12 columns.

What we do is to create a new media query within each grid item and change the
width as necessary. Take note we need to remove the margin-top property from
`',l-sidebar` as well.

    <p class="hljs-selector-class">.l-guest-article</p> {
      @<p class="hljs-keyword">include</p> clearfix;
      <p class="hljs-selector-class">.l-guest</p> {
        @<p class="hljs-keyword">media</p> (min-width: 700px) {
          <p class="hljs-attribute">width</p>: percentage(/);
          <p class="hljs-attribute">float</p>: left;
        }
    
        @<p class="hljs-keyword">media</p> (min-width: 1200px) {
          <p class="hljs-attribute">width</p>: percentage(/);
        }
      }
      <p class="hljs-selector-class">.l-main</p> {
        <p class="hljs-attribute">margin-top</p>: <p class="hljs-number">20px</p>;
        @<p class="hljs-keyword">media</p> (min-width: 700px) {
          <p class="hljs-attribute">width</p>: percentage(/);
          <p class="hljs-attribute">margin-top</p>: ;
          <p class="hljs-attribute">float</p>: left;
        }
        @<p class="hljs-keyword">media</p> (min-width: 1200px) {
          <p class="hljs-attribute">width</p>: percentage(/);
        }
      }
      <p class="hljs-selector-class">.l-sidebar</p> {
        <p class="hljs-attribute">margin-top</p>: <p class="hljs-number">20px</p>;
        @<p class="hljs-keyword">media</p> (min-width: 700px) {
          <p class="hljs-attribute">width</p>: percentage(/);
          <p class="hljs-attribute">float</p>: right;
        }
        @<p class="hljs-keyword">media</p> (min-width: 1200px) {
          <p class="hljs-attribute">width</p>: percentage(/);
          <p class="hljs-attribute">margin-top</p>: ;
        }
      }
    }
    

Here’s the codepen for the final layout we’ve created:

See the Pen [guest-article layout with fixed-gutter grid (final)][38] by Zell
Liew
([@zellwk][20]) on [CodePen][21].

(Oh, by the way, you can achieve these results with Susy too. Just remember to
set the[gutter-position][39] to `inside-static`)

## Wrapping up {#wrapping-up}

Wow. This is a long article. I think I died three times writing it. (Thanks for
reading it all the way. I hope you didn’t die three times reading it though!
😛).

As you can see in this article, the steps to creating a responsive grid system
are relatively straightforward. The parts that most people get mixed up are 
steps 5 (determining gutter position) and 8 (making layouts responsive
).

Step 5 is simple when you think through all the possible methods, and we’ve
thought them through together. Step 8, on the other hand, is solvable easily 
once you have enough practice with writing[mobile first css][37]

I hope this article has given you the knowledge to build your own responsive
grid system, and I hope to see you build a custom grid for your next project.

Till then!</article>

 [1]: https://zellwk.com/blog/designing-grids
 [2]: https://zellwk.com/blog/designing-grids/
 [3]: https://zellwk.com/blog/migrating-from-bootstrap-to-susy/
 [4]: https://zellwk.com/blog/from-html-grids-to-css-grids/
 [5]: http://gridbyexample.com
 [6]: img/box-sizing.jpg
 [7]: https://zellwk.com/blog/understanding-css-box-sizing/
 [8]: https://smacss.com
 [9]: https://twitter.com/snookca
 [10]: img/columns.png
 [11]: img/grid-break.gif
 [12]: img/grid-columns.gif
 [13]: https://css-tricks.com/all-about-floats/
 [14]: img/float-collapse.png
 [15]: img/combi.png
 [16]: img/pattern-1side-margin.png
 [17]: img/subpixel.png
 [18]: img/margin-side-last-child.png
 [19]: http://codepen.io/zellwk/pen/mAYqrL/
 [20]: http://codepen.io/zellwk
 [21]: http://codepen.io
 [22]: img/pattern-1side-gutter.png
 [23]: img/pattern-split-margin.png
 [24]: http://codepen.io/zellwk/pen/BLZJza/
 [25]: img/pattern-split-padding.png
 [26]: http://codepen.io/zellwk/pen/ORYzQV/
 [27]: img/bg-relationship.jpg

 [28]: https://zellwk.com/blog/designing-grids/#how-big-should-columns-and-gutters-be-

 [29]: https://zellwk.com/blog/responsive-grid-system/how-the-grid-responds-to-different-viewports
 [30]: https://css-tricks.com/examples/nth-child-tester/
 [31]: https://zellwk.com/blog/how-to-write-mobile-first-css/
 [32]: http://codepen.io/zellwk/pen/ALkyAA/
 [33]: https://github.com/oddbird/susy/issues/609
 [34]: img/grid-example.png
 [35]: http://codepen.io/zellwk/pen/pEmLzY/
 [36]: img/grid-responsive.png
 [37]: https://zellwk.com/blog/mobile-first-css/
 [38]: http://codepen.io/zellwk/pen/qaGvxm/
 [39]: https://zellwk.com/blog/susy-gutter-positions/
