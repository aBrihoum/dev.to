## Introduction

Building application/websites with Angular always come with a downside: **the bundle size**.
The latter has a direct impact on the loading speed & the user experience of our project.

![Angular bundle size](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nhxp0nujx68r66h83lm1.png)

Even if we finally reduced the bundle size, there are other boxes to check to have the ideal website.

Personally, I have four steps to follow when building apps/websites.

> 1. Designing
> 2. Coding
> 3. Making the website responsive
> 4. Optimizing

On this post, we will focus on the **last step**.

---

## How I optimized my Angular website

I'll start with the problems I've faced, then how I've addressed them.

### 1 - Visual Problems

The following [**link**](https://megapizza-angular-kxgk919qy-oubrdb.vercel.app/) is a showcase of my website after the **3rd** step.

{% vimeo 730481699 %}

From this video, we can extract four visual problems :

#### 1.1 - Visual problem 1

The website looks broken for a split second, then loads ordinarily

![Angular styles late loading](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z1rbi7es3ffdijdjemd3.jpg)

#### 1.2 - Visual problem 2 & 3

The font took ages to load, same thing with the pizza picture

![No font nor pizza picture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jtueaqi6vr4lhpym6tfr.jpg)

#### 1.3 - Visual problem 4

The speed of loading images is super slow.

---

### 2 - Invisible problems

Let's open the dev-console and see what's happening under the hood.

{% vimeo 730486349 %}

I can take out two issues from this video

#### 2.1 - Invisible problem 1

![dev-tools](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f9c0byifiw5rjvz3fd4j.png)

The website took **4.57s** to fully load, with **98 request** and **5.4 MB of resources**. To put those numbers into perspective, a 3g internet will take ~24s to load download all the resources

#### 2.2 - Invisible problem 2

![Broken loading tree](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9zv8pa80qn6g7zambgdf.png)

The pizza picture, took **~1.07s** (0.689s + 0.387s) to be displayed, this means the user was seeing a broken slider for 1 second. The same goes with the font.

> You may ask : Why did he pick the pizza from of all the pictures ?
> I'll answer you with another question :
> What picture do see first when you visit the website ?

---

### Lighthouse Score

![Lighthouse Score](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wv6wx6ic6um0hz6q39v6.png)

As I expected, the **LCP** (largest Contentful paint) and the **CLS** (Cumulative Layout Shift) are bad, because of [Invisible problem N°2](#invisible-problem-2) and [Visual problem N°1](#visual-problem-1) respectively, surprisingly the First Contentful Paint is good.

### Bundle size

![Bundle size](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/331z7rzri71pw6cu3mdx.png)

Not that bad, but we can do better.

ℹ️ **Note :** before explaining and fixing said problems, lets first **optimize** the bundle size.

---

### Improving the bundle size

Before starting, I would like to highlight something :

- ⚠️ **Never** import third party CSS inside any of your Angular components, instead use `styles.css`.

---

There's lots of ways on how to minimize the bundle size, but that's not my subject for today, here I'm showcasing how **'I'** optimized my `Angular` website.

#### 1 - Lazy load

The first thing I personally do is to **Lazy load _noncritical_ third party libraries**, this means libraries that are not required the second the website loads, therefore their load can be delayed until all of the more important resources are loaded. I'll give you an example to clarify more :

- I have a plugin called [lightGallery](https://www.lightgalleryjs.com/), the latter is only required when a user wants to open an image gallery. Logically, we can delay his load until all of the website more critical resources (like the images) are downloaded.

- Same thing with goes with `Bootstrap`, his `JavaScript` is only required when we need [interactivity](https://getbootstrap.com/docs/5.2/customize/optimize/#lean-javascript) in our project, like for example **:** Opening a modal, Using a collapse or a carousel… So we can delay his load too.

#### 1.1 - Lazy load LightGallery

In the following video, I'll explain in detail the process :

{% vimeo 731044477?texttrack=en %}

The code I used in the video :

```javascript
// main.component.ts
let src = "https://jsdelivr.com"
window.onload = () => {
  let script = document.createElement("script")
  script.src = src
  script.async = true
  document.head.appendChild(script)
}
```

#### 1.2 - Lazy load Bootstrap

The same process goes with `Bootstrap`, remember `jsdelivr` ? Search for 'bootstrap' and :

![jsdelivr](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6q2e8fih7zjs1ymn1j3c.png)

Copy the link and replace the old with the new one.

![main.ts](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/go7dc4lev58gwdyra9ar.png)

ℹ️ Ps : `Remember to remove any other imported Bootstrap JavaScript`

![Bootstrap](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/v6umr63mpaa9mkmqukro.png)

#### 1.3 - Bundle size

![Bundle size](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/692sylxbmb3nwgurnel3.png)

We eliminated (125.01 kB)

### 2 - Removing unused modules

My website is a `single-page website`, even though, `Angular routing` is installed. To fix this, all I need to do is to comment out `AppRoutingModule` on my `app.module.ts`

![app.module.ts](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2wmqjm3fs5uzrez5ua02.png)

Now, I need to replace `<router-outlet></router-outlet>` with my parent component selector, which is `app-main`

![app-main](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zok1lxu2b3ij8yo06jku.png)

#### 2.1 - Bundle size

![Bundle size](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ph9fwqugp47q7hc9dhc0.png)

We eliminated a total of (**201.31 kB**) from the initial build.

The [website](https://megapizza-angular-jfoncqyln-oubrdb.vercel.app/) after reducing the bundle size.

The lighthouse score improved a little, but the website still has all the problems mentioned earlier. **Now let's fix them**.

![Lighthouse](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/svkeerlhg52qia6dq5hb.png)

---

### Explaining [Visual problem N°1](#11-visual-problem-1)

This website is build with `Bootstrap`, and `styles.css` contains `Bootstrap's` CSS. The reason of this problem is that `Angular` started printing the website before `styles.css` finished downloading, this means we had no stylesheet for `Bootstrap` until `styles.css` finished downloading.

![styles.css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pmgmlosxk6kr2o375ifw.png)

To confirm this, we can try to block `styles.css` from downloading at all and see if we have the same results.

![styles.css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/anyi0e46ugzsozh669z5.png)

Yea, the same.

---

### Solving [Visual problem 1](#11-visual-problem-1)

To solve this problem, all my `critical` CSS needs to be ready when `Angular` start printing. Critical CSS means :

> `The CSS responsible for the content that's immediately visible when we open a website`.

or :

> `The CSS of the first page you see when opening a website`.

In my case :

- `Bootstrap CSS`
- `SiwperJS CSS`.

But because I have buttons in the **first page** that are styled with custom CSS, they also considered as `critical`.

![Buttons](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yekpdjmv8fpabpv2ug5w.png)

Same thing goes with the `animation` I'm using on my website, they're critical too, the video below explains everything :

{% vimeo 731100526?texttrack=en %}

To resume, we have as `Critical` :

> - `Bootstrap CSS`,
> - `SwiperJS CSS`,
> - `Custom CSS`,
> - `Animations CSS`,

Now, let's get back to work.

First, I created a SCSS file named `bootstrap.scss`, and I imported inside it only the [component I need](https://getbootstrap.com/docs/5.2/customize/optimize/)

![bootstrap.scss](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yzhxbnqpq1dafn3wbnfv.png)

ℹ️ Ps : you can import all of `bootstrap` if you you want, because later I'll explain how we can remove unused CSS using `PurgeCSS`.

And I did the same thing with `SwiperJs`, `Animations`, and my custom CSS

![css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0k804239jyijuinjw2b1.png)

Next, I created a file named `combined.scss` and imported all the SCSS files I just created

![combined.scss](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l5n0vyts1isc08lc9hza.png)

To clarify more, that's the list of files you should have :

![list](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nsu09hr20zaeds58twcw.png)

ℹ️ Ps : `don't forget to remove the old imported CSS, ex : don't import Bootstrap in both styles.scss and combined.scss`.

After that, I jumped to angular.json, and under `styles[]` :

```json
{
  "projects": {
    "app": {
      "architect": {
        "build": {
          "options": {
            "styles": []
          }
        }
      }
    }
  }
}
```

I added the following :

```json
{
  "input": "[YourPath]/combined.scss",
  "inject": false,
  "bundleName": "combined"
}
```

![angular.json](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l6d4w11m5pf5ng2nul08.png)

Then, I opened my `index.html` and added the code bellow on the top of the `<head>` tag

```html
<!-- index.html -->
<link rel="preload" href="combined.css" as="style" />
<link rel="stylesheet" href="combined.css" />
```

![index.html](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4bmije5bm3311pkgdbjt.png)

What I've done here, is that the second a user visits the website, the first resource to be added to the download queue is `combined.scss`, this means the browser will start downloading my website resources with `combined.scss` on the top of the list, thus when Angular starts printing, the critical CSS are already prepared.

![Preload](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wggvvo3163xz9yte66xo.png)

Source : [https://developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload)

#### Bundle size

![Bundle size](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ptvymr00nh00n14ucpvi.png)

After building, I had this `Lazy Chunk Files` section, with my `combined.css` file there, in reality, I'm preloading, not lazy loading it.

You can also notice that the size of `styles.css` dropped significantly

Now, thanks to `PurgeCSS`, I'll try to reduce the size of `combined.css` by removing unused CSS.

---

#### Installing PurgeCSS

On my command prompt :

```shell
# command prompt
npm i -D purgecss
```

After that, I created a file named `purgecss.config.js` in the root of my project with the following lines :

```javascript
// purgecss.config.js
module.exports = {
  content: ["./dist/**/index.html", "./dist/**/*.js"],
  css: ["./dist/**/combined.css"],
  output: "./dist/[FOLDER]/combined.css",
  safelist: [/^swiper/],
}
```

Ps : Replace `[FOLDER]` (within the `output` property).

![purgecss.config.js](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iq48tqt94kbub9wtdu8q.png)

**Ps:** you may notice that I have set `safelist` to `[/^swiper/]`, That's because I don't want `PurgeCSS` to remove any `SwiperJS` CSS, because `SwiperJS` will add some CSS classes that `PurgeCSS` don't know about **after** the page is executed, and this lead `PurgeCSS` to remove them.

Next, I opened `package.json`, and edited `build` from :

```json
"build": "ng build"
```

To :

```json
 "build": "ng build && npm run purgecss "
```

Then I created a new script named `purgecss` :

```json
"purgecss": "purgecss -c purgecss.config.js",
```

To clarify, that's how `package.json` should look like

![package.json](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/01ve8rcyuq56ufw7p8j7.png)

⚠️ **Note :**

To build, use `npm run build` instead of `ng build`, So `PurgeCSS` will kick in.

#### The result

{% vimeo 731391920?texttrack=en %}

Before and after using `PurgeCSS` in `combined.css` :

![combined.css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bfq6ehn1jgsciqqjmkdh.png)

Now, let's take a look on the loading tree :

![loading tree](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/15xooljalwy575bnsq2c.png)

Like I explained before, `combined.css` is now the first file in the download queue.
The downside of this method is that now we have two stylesheets (styles.css & combined.css), this means one more request to the server, and a couple of milliseconds wasted. Later I'll explain how I fixed this small issue.

#### Lighthouse

![Lighthouse](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8ev5vxidl93qzkf0d5o7.png)

Even if lighthouse is telling me : 'your website is perfect', he is not 100% correct,
What about the [Visual problem 2 & 3](#12-visual-problem-2-amp-3) & all the invisible problems?

[The website after this method](https://megapizza-angular-etb14gx4o-oubrdb.vercel.app/)

---

### Explaining [Visual problem 2 & 3](#12-visual-problem-2-amp-3)

The cause of this problem, is the loading tree, or the order of resources in the download queue.
Like you maybe already know, browsers have a limit of parallel requests.

![Chrome limit](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3nrvkk7t5fafn59qq12e.png)

![Loading tree](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/412ucweumviaukm8b935.png)
Source : [blog.bluetriangle.com](https://blog.bluetriangle.com/blocking-web-performance-villain)

Because of that, I need to prioritize the resources I need the most. This means I need to have the font & the pizza picture downloaded before other low-priority resources.

![loading tree](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ulelhddwy8ahuvahsj1d.png)

### Solving [Visual problem 2 & 3](#12-visual-problem-2-amp-3)

This problem is easy to solve, all I need is to preload (like we did earlier) the font, and the pizza picture.

Now, I should know the font used on the first page. After that, inside the `<head>` of my `index.html` I added :

```html
<!-- index.html -->
<link
  rel="preload"
  href="[YourPath]/DayburyRegular.woff2"
  as="font"
  type="font/woff2"
  crossorigin
/>
```

The same thing for the pizza picture :

```html
<!-- index.html -->
<link rel="preload" href="[YourPath]/pizza.webp" as="image" />
```

It should look like this :

![index.html](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lknnvxda2q12ja4ikl8v.png)

⚠️ **Note :**

Go to the CSS file containing your `@font-face` :

![fonts.css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uy7j4j2ly5md1yct6str.png)

If the path of your font is different from the path you put on your index.html, change it to the same as index.html, **Even if they lead to the same file**, they must be written the same.

![fonts.css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s38n3g958h8j79nwz24y.png)

Otherwise the browser will re-download the font.

![Fonts](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/njuq3joamhksey2931a3.png)

For the last step, the browser needs to know the `font-family` of my preloaded font, which is : `'DayburyRegular.woff2'` before `Angular` starts printing the website.

When we say 'before `Angular` starts printing' we say : `combined.scss`. All I have to do now is to transfer my `font-family` to `combined.scss`.

I created a file named `pre-fonts.scss`, and I transferred my font from the old SCSS to the new one

![fonts](https://res.cloudinary.com/practicaldev/image/fetch/s--dyTHLs7B--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://im2.ezgif.com/tmp/ezgif-2-6d3e1da4f5.webp)

After that, I imported `pre-fonts.scss` into `combined.scss`

![combined.css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4johjal46iz7yvxpxc4e.png)

#### The result

Let's [visit the website](https://megapizza-angular-ckx3ks30t-oubrdb.vercel.app/) after this fix, and check the resources.

![Download tree](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ar00bf6o4ned8qgmpdim.png)

Now, the font and pizza won't get late to the party again.

---

### Explaining [Visual problem 4](#13-visual-problem-4)

{% vimeo 731544881 %}

Like I explained in the video, when we lazy load resources that are not visible in the viewport, the other resources (that are visible) will load faster, because now we are downloading for example 10 images, instead of 100.

ℹ️ Globally : we only need to load pictures that are actually needed, instead of loading them up front.

### Solving [Visual problem 4](#13-visual-problem-4)

The solution is to implement `Lazy Loading`. There are many methods and techniques, but my personal choice is to go with [lazysizes](https://github.com/aFarkas/lazysizes) By Alexander Farkas.

---

But before, because we have all our critical CSS inside `combined.css`, let's take a look at my `styles.css`

![styles.css](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jst3vdxj177zj5t47sof.png)

Poor file, look so empty, Jokes aside, since All CSS inside it are noncritical, I better lazy load it with the same method I used when I [**Lazy loaded LightGallery**](#11-lazy-load-lightgallery).

First, I jumped to `angular.json`, under `styles[]` I edited :

```json
"src/styles.scss"
```

to

```json
{
  "input": "src/styles.scss",
  "inject": false,
  "bundleName": "styles"
}
```

![angular.json](https://res.cloudinary.com/practicaldev/image/fetch/s--fBFq9CP8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://im2.ezgif.com/tmp/ezgif-2-5e61b04435.webp)

Now, I need to to load `styles.css` after all the resources finished downloading ( like I did when I [**Lazy loaded LightGallery**](#11-lazy-load-lightgallery))

Alright, inside my `main.component.ts`, under `ngAfterContentInit()` and within `window.onload` I added :

```javascript
var link = document.createElement("link")
link.rel = "stylesheet"
link.type = "text/css"
link.href = "styles.css"
document.head.appendChild(link)
```

![main.ts](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/00cwz3x016ab6fzbfwcs.png)

---

Now let's get back to the problem. I would be very pleased to explain to you how I lazy loaded my website, but this post is already long enough, plus, that's not the main subject. So I'll jump directly to the results, however, I'm planning on writing a detailed step by step guide, and link it right here.

#### The result

{% vimeo 731774246?texttrack=en %}

![result](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b1eg60msnxw7c40x16ix.png)

#### Lighthouse

![Lighthouse](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7c5zjgrlyiq1opphj5nw.png)

Mission accomplished ✅

ℹ️ **Note :** While fixing my visual problems, all my invisible problems are solved too :

- [Invisible problem 1](#21-invisible-problem-1) was fixed when we solved [Visual problem 4](#solving-visual-problem-4), (when I lazy loaded the website with `lazysizes`)

- [Invisible problem 2](#22-invisible-problem-2) was fixed when we solved [Visual problem 2 & 3](#solving-visual-problem-2-amp-3) (when I preloaded the font, and the pizza picture)

You can visit the [website](https://megapizza-angular-d1qw82z6o-oubrdb.vercel.app/) after this last step (:

## The Take

- When your website start printing, make sure that all of your critical CSS is ready.

- If necessary, preload some of your resources (the main ones), for a better UX (like we did with the pizza picture & the font).

- Always, like Always lazy load your images, and if possible, lazy load your uncritical JS & CSS.

- Try to install the minimum of third-party libraries, and uninstall the useless ones.

- Always open your dev-console, and analyze and prioritize the order of your resources.

---

You may find mistakes related to my English, maybe I was wrong about some of the things I said, or the way I explained them. Your suggestions and advices are always more than welcome.

ℹ️ **Note :** I tried to be beginner friendly as possible, which is why you find me a little repetitive and boring in some cases.
