## Introduction

Building applications/websites using `Angular` has a downside - **the bundle size**. This directly affects the loading speed and user experience of our projects.

![Angular bundle size](https://i.ibb.co/gwpLFzP/huge-bundle-size.webp)

Reducing **the bundle size** is important, but there are other essential elements to consider for creating an ideal website.

Personally, I follow a four-step process when building apps/websites:

1.  Designing
2.  Coding
3.  Ensuring Responsiveness
4.  Optimizing

In this post, we will focus on the **last step**.

---

## Getting started

I'll start by discussing the problems I encountered and how I addressed them, including the steps I took to reduce the bundle size.

### 1. Visual Problems

The following **[link](https://megapizza-v3-91y77whep-brihoum.vercel.app/)** showcases my website after the **3rd step**.

[![website](https://img.shields.io/badge/Vercel-Visit%20the%20website-green?style=for-the-badge&logo=vercel)](https://megapizza-v3-91y77whep-brihoum.vercel.app/)

{% youtube 2UMDps3k3Qs %}

From this video, I can **identify** `3` visual problems :

#### 1.1. Visual Problem 1

The website looks broken for a split second, then loads normally.

[![Angular styles late loading](https://i.ibb.co/S7fhcVB/broken-website.webp)](https://i.ibb.co/S7fhcVB/broken-website.webp)

#### 1.2. Visual Problems 2 & 3

The `font` took ages to load, the same thing goes with the `pizza picture` and the other more important resources

[![No font for the pizza picture](https://i.ibb.co/7JYpLd7/slow-font-pizza-load.webp)](https://i.ibb.co/7JYpLd7/slow-font-pizza-load.webp)

---

### 2. Invisible problems

Let's open the dev console and see what's happening under the hood.

{% youtube kCETm6GuYmQ %}

I can **identify** two issues from this video.

#### 2.1. Invisible Problem 1

[![dev-tools](https://i.ibb.co/JtT4v5Q/dev-tools.webp)](https://i.ibb.co/JtT4v5Q/dev-tools.webp)

The website took **6.84s** to fully load, with **109 requests** and **4.4 MB of resources**. This happened because the website **loaded all its resources** from pages 1 to 5, including unnecessary ones.

To put those numbers into perspective, it would take a **3G internet** connection about **~24s** to load all the resources.

#### 2.2. Invisible Problem 2

[![bad loading tree](https://i.ibb.co/Jr3LCGm/late-pizza-load.webp)](https://i.ibb.co/Jr3LCGm/late-pizza-load.webp)

The website is loading resources that are **not needed initially**, before loading the necessary ones, **causing a delay** in rendering critical resources.

For example :

- The website first loaded all the used backgrounds, from page 1 to 5 (`number.3`).

- Then, a picture located on page 4, which was not yet needed, was loaded (`number.4`).

- Next, 7 other pictures located on page 2, also not yet needed, were loaded (`number.5`).

- Lastly, **the most critical resource**, the `pizza picture` was loaded.

So, the `pizza picture` took approximately **~2.129s** (`1.92s + 0.209s`) (`number.1 & 2`) to load, resulting in a broken slider being displayed to the user during this time.

The same goes for the `font`.

[![late font loading](https://i.ibb.co/ByVrDM3/late-font.webp)](https://i.ibb.co/ByVrDM3/late-font.webp)

It was the last resource to load, taking approximately **~4.09s** (`1.72s + 2.37s`) to render.

ℹ **Note:**

You may ask: Why did he pick the `pizza picture` from all the pictures ❓

I'll answer you with another question :

What picture do you see first when you visit the website ❓

---

### Lighthouse Score

[![Lighthouse Score](https://i.ibb.co/xz7M9S8/lighthouse-0.webp)](https://i.ibb.co/xz7M9S8/lighthouse-0.webp)

As expected, the **LCP** (largest Contentful paint) and the **CLS** (Cumulative Layout Shift) are bad, due to the [Invisible Problem 2](#22-invisible-problem-2) and [Visual Problem 1](#11-visual-problem-1) respectively, surprisingly the **FCP** (First Contentful Paint) is decent.

### Bundle size

[![Bundle size](https://i.ibb.co/Z1LmKs3/build-0.webp)](https://i.ibb.co/Z1LmKs3/build-0.webp)

We can do much much better.

✳ **Before** explaining and fixing said problems, let's first **optimize** the bundle size.

---

### Improving the bundle size 📉

I would like to highlight something before starting :

- ⚠️ **Never** import third-party styles (CSS/Sass...) inside any of your Angular components, instead use the global `styles.scss` file.

There are many ways to reduce the bundle size, but that's not the focus of this article, here I will be showcasing how **'I'** optimized my `Angular` website.

#### 1. Lazy loading third-party libraries

The first thing I personally do is to **Lazy Load _noncritical_ third-party libraries**. This means libraries that are **not required** the second the website loads, therefore their load **can be delayed** until all the more important resources are loaded and rendered. This reduces the main bundle size, which in turn improves the website's loading speed.

I'll give some examples to clarify more :

- I have a plugin called **[lightGallery](https://www.lightgalleryjs.com/)**, which is only needed when a user wants to open an image gallery. Logically, its load can be delayed until the more critical resources of the website (such as the pizza picture and important styles/fonts) are downloaded and rendered in the view.

- I also have **[Bootstrap](https://getbootstrap.com/)** installed. its `JavaScript` is only required when we need **[interactivity](https://getbootstrap.com/docs/5.2/customize/optimize/#lean-javascript)** in our project, like for **example:** opening a modal, Using a collapse, a carousel… So we can delay its load too.

That's the list of **non-critical third-party libraries** used by the website :

- **[lightGallery](https://www.lightgalleryjs.com/)**.
- **[Bootstrap](https://getbootstrap.com/)**.
- **[node-snackbar](https://github.com/polonel/SnackBar)**
- **[Popper.js](https://popper.js.org/)**
- **[Wow.js](https://wowjs.uk/)**

Here, in the video below, I'll explain how to lazy load them.

{% youtube 7quStWqNwaw %}

**⚠ Note:** I made a small mistake on the video ([1:30](https://youtu.be/7quStWqNwaw?t=90)), it should be

> "input": "node_modules/wow.js/dist/wow.js",

&

> appendScript('wow.js');

ℹ The code I used on the video :

```typescript
// app.component.ts
function appendScript(name: string) {
  let script = document.createElement("script");
  script.src = name;
  script.async = true;
  document.head.appendChild(script);
}
```

To summarize 📝 :

- We instructed `Angular` to load our scripts **as separate files** during the build and **not inject them**, so they can be `lazy-loaded`.

- Then, we used `window.onload` event to load our scripts after the entire website, including its content such as images and styles, has loaded. This approach ensures that our scripts are the last resources to load.

##### 1.2. Bundle size

[![Bundle size 1](https://i.ibb.co/8P23JLN/build-1-1.webp)](https://i.ibb.co/8P23JLN/build-1-1.webp)

✅ Just like that, we eliminated (**163.02 kB**).

#### 2. Removing unused modules

This website has no routes, as it only contains a single page, even though the `RouterModule` is installed. We need to remove it because it's basically useless.

On the `app.module.ts` file, we remove `RouterModule` from the imports array:

[![app.module.ts](https://i.ibb.co/4JQD7TD/remove-angular-router.webp)](https://i.ibb.co/4JQD7TD/remove-angular-router.webp)

##### 2.1. Bundle size

[![Bundle size 2](https://i.ibb.co/zrNkGQX/build-2.webp)](https://i.ibb.co/zrNkGQX/build-2.webp)

✅ We were able to eliminate (**75.85 kB**) by removing `RouterModule`.

This means we eliminated a total of (**238.71 kB**) from the initial build.

▶ You can check the [website](https://megapizza-v3-2qq0b0oth-brihoum.vercel.app/) after reducing the bundle size.

[![website](https://img.shields.io/badge/Vercel-Visit%20the%20website-green?style=for-the-badge&logo=vercel)](https://megapizza-v3-2qq0b0oth-brihoum.vercel.app/)

#### Lighthouse Score

The website's Lighthouse score **has slightly improved**, but all the previously mentioned **issues still persist**. Therefore, we need to address them to optimize the website's performance further.

[![Lighthouse](https://i.ibb.co/5j9q23F/lighthouse-1.webp)](https://i.ibb.co/5j9q23F/lighthouse-1.webp)

---

### Explaining [Visual Problem 1](#11-visual-problem-1)

This website uses `Bootstrap` and `styles.css` contains `Bootstrap's` CSS and other **important styles**. The reason for this problem is that `Angular` started rendering the website before `styles.css` finished downloading, causing the page to be displayed without the necessary styles.

[![styles.scss](https://i.ibb.co/CmXRgw9/scss.webp)](https://i.ibb.co/CmXRgw9/scss.webp)

To verify this, we can test by blocking the download of `styles.css` completely and observe if we still get the same results :

[![no styles.css](https://i.ibb.co/SK0br0x/no-styles.webp)](https://i.ibb.co/SK0br0x/no-styles.webp)

Yes, we do.

---

### Solving [Visual Problem 1](#11-visual-problem-1)

To solve this issue, we need to ensure that all **critical CSS** is loaded before `Angular` starts rendering the website.

**Critical CSS** means :

> `The CSS responsible for the content that's immediately visible when we open a website`.

or :

> `The CSS of the first page you see when opening a website`.

How can we know ❓ well, watch the video below :

{% youtube OpEXgol3aKs %}

So, we have as **critical** the following :

- `Bootstrap CSS`;
- `SwiperJS CSS`;
- `Custom CSS`;
- `Animations`;

**⚠ Note:** The `font` file is also critical, however, we will address it with [Visual Problems 2 & 3](#12-visual-problem-2-amp-3). This is because the font file contains `3` fonts, whereas the homepage requires only `1`. For now, we consider it non-critical.

And as **non-critical** :

- `Snackbar CSS`
- `lightGallery CSS & its plugins`
- `fonts file`

Now, Instead of using a single file (like `styles.scss`) containing all styles, we can separate them based on their priority.

We create two files: `pre_styles.scss` & `late_styles.scss` and import our **critical** and **non-critical** styles into them, respectively.

[![late & pre styles](https://i.ibb.co/RCczdbT/late-and-pre-styles.webp)](https://i.ibb.co/RCczdbT/late-and-pre-styles.webp)

If the `.gif` is not clear, that's the content of the two files (`styles.scss` is completely empty):

[![late & pre styles](https://i.ibb.co/s1rr4rQ/pre-late-styles.webp)](https://i.ibb.co/s1rr4rQ/pre-late-styles.webp)

**ℹ Note:** Instead of importing the entire Bootstrap library, **you can** import only the specific [components that you need](https://getbootstrap.com/docs/5.3/customize/optimize/).

Now, I need to **preload** `pre_styles.scss` & **lazy load** `late_styles.scss`.

#### 1. Preloading critical styles

Just like we did before when we **[lazy loaded third-party libraries javascript](#1-lazy-loading-third-party-libraries)**, we need to tell angular to load `pre_styles.scss` **as a separate file** during the build and **not inject it**, so we can **preload** it.

Inside `angular.json`, under the `styles[]` array, we remove the default value :

```json
// angular.json
"styles": ["src/styles.scss"],
```

and change it to :

```json
// angular.json
  "styles": [
    {
      "input": "src/pre_styles.scss",
      "inject": false
    },
    {
      "input": "src/late_styles.scss", // <- we will use it later.
      "inject": false
    }
  ]
```

[![angular.json separate file](https://i.ibb.co/Y2fY9RK/angular-json-pre-and-late-styles-10fps.webp)](https://i.ibb.co/Y2fY9RK/angular-json-pre-and-late-styles-10fps.webp)

After we build, we can locate our two files, `pre_styles.scss` and `late_styles.scss`, inside the `dist/` folder.

[![styles](https://i.ibb.co/g4kDvFJ/ls-la.webp)](https://i.ibb.co/g4kDvFJ/ls-la.webp)

Now, in order to **preload** `pre_styles.scss`, we add the following code inside the `<head>` element of `src/index.html`:

```html
<!-- index.html -->
<head>
  ...
  <link rel="preload" href="pre_styles.css" as="style" />
  <link rel="stylesheet" href="pre_styles.css" />
</head>
```

[![preload styles](https://i.ibb.co/Pw2gLrQ/pre-styles.webp)](https://i.ibb.co/Pw2gLrQ/pre-styles.webp)

What I've done here is **prioritize** the loading of `pre_styles.css` as the **first resource** in the download queue. This means the browser will start downloading the website resources with `pre_styles.css` at the **top of the list** so that when `Angular` starts rendering, the `critical styles` are already loaded and ready.

You can read more about `rel=preload` : [https://developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload)

#### 2. Lazy loading non-critical styles

In the previous step, we **already** told angular to load `late_styles.scss` **as a separate file**, it's time to use it.

Within the `ngAfterContentInit()` method of `app.component.ts`, **we define a new function** named `appendStyle()` below the `appendScript()` function that we created earlier :

```typescript
// app.component.ts
ngAfterContentInit() {
  //...
  function appendScript() {
    //...
  }
  function appendStyle(name: string) {
    let style = document.createElement("link");
    style.rel = "stylesheet";
    style.type = "text/css";
    style.href = name;
    document.head.appendChild(style);
  }
}

```

Now, we use `appendStyle()` function to **lazy load** `late_styles.scss` by calling it within the `window.onload` event :

```typescript
// app.component.ts
  ngAfterContentInit() {
    //...
    window.onload = () => {
      //...
      appendStyle('late_styles.css');
    };
  }
```

[![lazy loading styles](https://i.ibb.co/rkZjGtv/lazy-styles.webp)](https://i.ibb.co/rkZjGtv/lazy-styles.webp)

**ℹ A reminder:** The `window.onload` event is fired when the entire website loads.

Now, we restart the server and observe :

[![order of resources](https://i.ibb.co/2nrGsgP/pre-and-late-styles-loading-order.webp)](https://i.ibb.co/2nrGsgP/pre-and-late-styles-loading-order.webp)

We can see that `pre_styles.css` is the **first file to load**, providing all the `critical styles` necessary for `Angular` to begin rendering, eliminating the broken website look.

`late_styles.css` is among the **last files to load**, making room for more `critical styles` to load faster.

✅ We completed our task, and **[Visual Problem 1](#11-visual-problem-1)** is now fixed.

❗ However, we may have a tiny problem :

[![large file size](https://i.ibb.co/hZnrJ9p/large-pre-styles.webp)](ttps://i.ibb.co/hZnrJ9p/large-pre-styles.webp)

`pre_styles.css` **is quite large**, and a significant portion of it contains **dead code that is never used**. This is mainly because we imported the entire `Bootstrap` library instead of [selectively importing the required components](https://getbootstrap.com/docs/5.3/customize/optimize/).

✅ Using `purgeCSS`, we can eliminate all the unused code and optimize the performance further.

#### 3. Installing PurgeCSS

On the command prompt :

```shell
# command prompt
npm i -D purgecss
```

Now, we create a new file named `purgecss.config.js` on the root of the project, and add the following code :

```javascript
// purgecss.config.js
module.exports = {
  content: ["./dist/**/index.html", "./dist/**/*.js"],
  css: ["./dist/**/combined.css"],
  output: "./dist/[FOLDER]/combined.css",
  safelist: [/^swiper/],
};
```

**⚠ Note**: Replace `[FOLDER]` with your app name (within the `output` property).

[![purgeCSS with angular](https://i.ibb.co/FB6HCNq/purgecss.webp)](https://i.ibb.co/FB6HCNq/purgecss.webp)

**ℹ Note**: You may have noticed that I have set the `safelist` to `[/^swiper/]`. This is because I want to prevent `PurgeCSS` **from removing any CSS related** to `SwiperJS`. Since `SwiperJS` adds CSS classes dynamically after the page load, `PurgeCSS` may not be aware of them and could mistakenly remove them.

Next, we navigate to `package.json` and create a new script named `purgecss` :

```json
"purgecss": "purgecss -c purgecss.config.js",
```

Then, we edit the `build` script from :

```json
"build": "ng build"
```

To :

```json
 "build": "ng build && npm run purgecss "
```

[![package.json](https://i.ibb.co/MCnVRBj/package.webp)](https://i.ibb.co/MCnVRBj/package.webp)

**⚠️ Note:** Build using `npm run build` instead of `ng build`, so `purgecss` script kicks in.

After building, the size of `pre_styles.css` dropped by (**190.6 kB**) :

[![pre_styles.css purged](https://i.ibb.co/4Ss4zXf/purged-pre-styles.webp)](https://i.ibb.co/4Ss4zXf/purged-pre-styles.webp)

#### Lighthouse

[![Lighthouse 22](https://i.ibb.co/dpyxvSF/lighthouse-2.webp)](https://i.ibb.co/dpyxvSF/lighthouse-2.webp)

The **CLS** (Cumulative Layout Shift) has decreased, because, as expected, we fixed **[Visual Problem 1](#11-visual-problem-1)**. However, **LCP** (largest Contentful paint) is still poor, because the `pizza picture` is always `late` to the party, AKA **[Visual Problems 2 & 3](#12-visual-problem-2-amp-3)**.

[![bad LCP](https://i.ibb.co/zPkLXny/LCP.webp)](https://i.ibb.co/zPkLXny/LCP.webp)

▶ Visit the [website](https://megapizza-v3-3ndn8z08x-brihoum.vercel.app/) after this step :

[![website](https://img.shields.io/badge/Vercel-Visit%20the%20website-green?style=for-the-badge&logo=vercel)](https://megapizza-v3-3ndn8z08x-brihoum.vercel.app/)

---

### Explaining [Visual Problems 2 & 3](#12-visual-problem-2-amp-3)

The cause of this problem is the **order** of resources in the download queue.
As you may already know, browsers have a limit on the number of parallel requests.

[![Chrome limit of requests](https://i.ibb.co/wJW14x5/limit-of-requests-2.webp)](https://i.ibb.co/wJW14x5/limit-of-requests-2.webp)

Source: [blog.bluetriangle.com](https://blog.bluetriangle.com/blocking-web-performance-villain)

Given this limitation, we must **prioritize** the **order** in which resources are loaded, making sure to load **high-priority** resources like the `font` and `pizza picture` before the lower-priority ones

### Solving [Visual Problems 2 & 3](#12-visual-problem-2-amp-3)

This issue can be easily resolved by **preloading** the `font` and the `pizza picture`, as we did earlier.

Inside the `<head>` element of `src/index.html`, we add the following code :

```html
<!-- index.html -->
<head>
  ...
  <link rel="preload" href="[YourPath]/pizza.webp" as="image" />
</head>
```

And we follow the same procedure with **the font used on the home page** (`DayburyRegular.woff2`) :

```html
<!-- index.html -->
<head>
  ...
  <link
    rel="preload"
    href="[YourPath]/DayburyRegular.woff2"
    as="font"
    type="font/woff2"
    crossorigin="anonymous"
  />
</head>
```

[![index.html](https://i.ibb.co/HVr2SD0/pre-font.webp)](https://i.ibb.co/HVr2SD0/pre-font.webp)

**⚠ Note:** The use of `crossorigin` here is important.

Now, I need to transfer my `font` from `fonts.css` to `pre_styles.scss`

[![font](https://i.ibb.co/RYBzL3j/font.webp)](https://i.ibb.co/RYBzL3j/font.webp)

**⚠️ Note:** Make sure to use the correct path for the font `src` when transferring it.

[![load](https://i.ibb.co/pyCRbHn/correct-loading.webp)](https://i.ibb.co/pyCRbHn/correct-loading.webp)

✅ The task has been completed and the issue causing **[Visual Problems 2 & 3](#12-visual-problem-2-amp-3)** has been resolved. This means no more delays in loading the `font` or the `pizza picture`.

▶ Visit the [website](https://megapizza-v3-git-unoptimizedversiondevto-brihoum.vercel.app/) after this step :

[![website](https://img.shields.io/badge/Vercel-Visit%20the%20website-green?style=for-the-badge&logo=vercel)](https://megapizza-v3-git-unoptimizedversiondevto-brihoum.vercel.app/)

---

### Explaining [Invisible Problem 1](#21-invisible-problem-1)

This issue occurs because the website **loads all its resources** from pages 1 to 5, even those that are **not necessary**.
Ideally, we should **only load** resources that are **currently visible** in the viewport and **postpone** the loading of the remaining resources until we **scroll to them**.
By following this approach, we **accelerate** the loading time of the visible resources by **reducing** the number of files that need to be downloaded initially.

**ℹ️ TL;DR:** We should only load resources that are visible on the screen.

### Solving [Invisible Problem 1](#21-invisible-problem-1)

The solution to this issue is to use a library that **loads only the visible resources and lazy loads the rest**. My personal choice would be [lazysizes](https://github.com/aFarkas/lazysizes) By Alexander Farkas.

I would be happy to explain how I implemented `lazy loading` on my website using [lazysizes](https://github.com/aFarkas/lazysizes), but this post is already long enough and that's not the primary focus for today. Instead, **I will jump directly to the results**. However, I am planning to create a detailed guide with step-by-step instructions, which I will link here for reference.

---

I implemented `lazy loading` for all the pictures and backgrounds on the website. The image below shows the difference before and after the implementation :

[![dev tools 2](https://i.ibb.co/q9RfdHw/dev-tools-2.webp)](https://i.ibb.co/q9RfdHw/dev-tools-2.webp)

✅ The website made only `49` requests initially, transferred `1.1 MB` of resources, and loaded all of them in just `1.28s`.

**ℹ Note:** The results mentioned above were achieved without utilizing the browser cache as it was disabled during the test.

And by caching the resources, the load time can be reduced to a maximum of ~ `0.5s`.

[![dev tools 2 - cache](https://i.ibb.co/CtHnmW8/dev-tools-cache.webp)](https://i.ibb.co/CtHnmW8/dev-tools-cache.webp)

Now, resources will be loaded as we scroll to them :

[![load on scroll](https://i.ibb.co/37NVWKX/load-on-scroll.webp)](https://i.ibb.co/37NVWKX/load-on-scroll.webp)

✅ The issue causing **[Invisible Problem 1](#21-invisible-problem-1)** has been successfully resolved.

▶ Visit the [website](https://megapizza-v3-qastti5tm-brihoum.vercel.app/) after this step :

[![website](https://img.shields.io/badge/Vercel-Visit%20the%20website-green?style=for-the-badge&logo=vercel)](https://megapizza-v3-qastti5tm-brihoum.vercel.app/)

#### Lighthouse score

[![Lighthouse](https://i.ibb.co/cy4GsH0/lighthouse-3.webp)](https://i.ibb.co/cy4GsH0/lighthouse-3.webp)

[![Lighthouse report badge](https://img.shields.io/badge/lighthouse-view%20report-blue?style=for-the-badge&logo=lighthouse)](https://googlechrome.github.io/lighthouse/viewer/?gist=58ba19c693d8313bd2d4d486f6505e09)

Mission accomplished ✅.

The load is fast as ⚡.

ℹ️ **Note:** [Invisible Problem 2](#22-invisible-problem-2) was also resolved when we fixed the [Visual Problems 2 & 3](#solving-visual-problem-2-amp-3) issues.

### MORE SPEED!

What if I told you we can **reduce** the bundle size **even more** and **enhance** the already impeccable `Lighthouse` score ❓

✳ The method I'm about to show you will allow us to initially **load only the first visible page**, and then **lazy load** the remaining pages afterward.

This means that instead of loading the entire website, we only load the first page component, resulting in a **smaller initial page size** and faster loading speed.

**ℹ️ TL;DR:** We only **load components** that are currently in view, and **lazy load** the remaining.

**⚠ Note:** Keep in mind this method comes with several downsides.

---

To start, Inside my `app.component.ts`, I need to remove all `pages` except for `page 1` from **rendering**, meaning only `page 1` can be loaded and displayed.

After that, I need to add a new `<ng-container>` and declare a `template Variable` named `#injectHere` inside it.

[![lazy load components](https://i.ibb.co/k3NkccD/remove-all-pages.webp)](https://i.ibb.co/k3NkccD/remove-all-pages.webp)

Next, in order to access `<ng-container>`, we declare the following :

```typescript
@ViewChild('injectHere', { read: ViewContainerRef }) injectHere!: ViewContainerRef;
```

And below, we define a new method that will be responsible for loading the remaining components :

```typescript
  async loadComponents() {
    const { Page2Component } = await import('./components/page2/page2.component');
    this.injectHere.createComponent(Page2Component);
    const { Page3Component } = await import('./components/page3/page3.component');
    this.injectHere.createComponent(Page3Component);
    const { Page4Component } = await import('./components/page4/page4.component');
    this.injectHere.createComponent(Page4Component);
    const { Page5Component } = await import('./components/page5/page5.component');
    this.injectHere.createComponent(Page5Component);
  }
```

Now, we need to call this method. In my opinion, it is better to wait for page 1 to fully load before loading the remaining components to ensure a faster initial loading speed. To achieve this, we call the method under the `window.onload` event :

```typescript
window.onload = async () => {
  await this.loadComponents();
  //...
};
```

**ℹ Note:** `loadComponents()` is _asynchronous_ and should be awaited before proceeding further.

[![lazy-load-components](https://i.ibb.co/yXcTKPy/lazy-load-components.webp)](https://i.ibb.co/yXcTKPy/lazy-load-components.webp)

And if we open the dev-console :

[![angular lazy load components](https://i.ibb.co/J5TQc2P/lazy-load-components.webp)](https://i.ibb.co/J5TQc2P/lazy-load-components.webp)

▶ Visit the [website](https://megapizza-v3-r9p12zc31-brihoum.vercel.app/) after this step :

[![website](https://img.shields.io/badge/Vercel-Visit%20the%20website-green?style=for-the-badge&logo=vercel)](https://megapizza-v3-r9p12zc31-brihoum.vercel.app/)

#### Bundle size

[![bundle size](https://i.ibb.co/JFnmdc1/build-3.webp)](https://i.ibb.co/JFnmdc1/build-3.webp)

✅ The main bundle size was reduced by (**83.29 kB**), resulting in a faster loading time and quicker display of page 1.

#### Lighthouse Score

[![lighthouse score](https://i.ibb.co/4ptV9H4/lighthouse-4.webp)](https://i.ibb.co/4ptV9H4/lighthouse-4.webp)

[![Lighthouse report badge](https://img.shields.io/badge/lighthouse-view%20report-blue?style=for-the-badge&logo=lighthouse)](https://googlechrome.github.io/lighthouse/viewer/?gist=4ebba9250e9b7e950151e9cf8c6232cb)

We have achieved a slight improvement by gaining some milliseconds and completely eliminating the blocking time.

---

## The Takeaways

- Make sure that all of your **critical styles are ready** when your website starts rendering.

- If necessary, **preload** some of your main resources, like we did with the `pizza picture` and the `font`.

- Always, like always **lazy load** your images, and if possible, lazy load your **non-critical** JS & CSS.

- Install the minimum required third-party libraries and uninstall any unused ones.

- Always open your dev console, analyze and prioritize the order of your resources.

---

If you have suggestions or advice regarding the information I shared, please don't hesitate to let me know. Your feedback is always welcome.

I aimed to explain everything in a beginner-friendly way, which may have led to some repetition.

You can find the website's source code on `Github` if you're interested:

[![MegaPizza Github](https://img.shields.io/badge/github-view%20on%20github-blue?style=for-the-badge&logo=github)](https://github.com/aBrihoum/megapizza_website)
