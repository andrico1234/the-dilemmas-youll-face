# The Dilemmas You'll Face When Creating a Component Library

> Note: This is a refreshed version of an article series I wrote a couple of years back, but the content is still very relevant.

Building a component library is a challenging and rewarding experience, but there’s more to it than writing great-looking components. At every step of the journey, you’ll need to make choices to ensure your library is built appropriately for its intended audience.

When building your library, you'll need to consider the following dillemas:

- [Should I write vanilla web components or use a library?](#should-i-write-vanilla-web-components-or-use-a-library)
- [Should I export my library as a single package or as scoped packages?](#should-i-export-my-library-as-a-single-package-or-as-scoped-packages)
- [How should I structure the repo of my library?](#how-should-i-structure-the-repo-of-my-library)
- [Should I use tooling to build and/or bundle my library?](#should-i-use-tooling-to-build-andor-bundle-my-library)
- [Aside: Application-level concerns vs library-level concerns](#aside-application-level-concerns-vs-library-level-concerns)
- [How can I safely publish my library?](#how-can-i-safely-publish-my-library)
- [What should my library’s testing strategy be?](#what-should-my-librarys-testing-strategy-be)
- [How to write documentation for my component library](#how-to-write-documentation-for-my-component-library)
- [What’s next?](#whats-next)

While this series is focused on building a component library using web components, many of the dilemmas I cover here can be applied to other web frameworks.
If you’d like to build your very first web component library, then check out [Component Odyssey](https://component-odyssey.com/), a course that teaches you everything you need to know to build, style, and publish a web component library that works in any framework.

## Should I write vanilla web components or use a library?

> Note: You might think that this section isn’t applicable if you don’t plan on using web components, but web components are interoperable with every other web framework! There are heaps of great libraries designed to make writing web components more pleasant, so it’s worth considering them for a future project.

Web Components are notorious for having a verbose API and involving confusing concepts like the shadow DOM. Writing web components without a helper library can be tricky, especially if you’re unfamiliar with them. Fortunately, there are heaps of ways to write web components, which can be grouped into 3 levels of abstractions:

1. Use an existing component library as a base and theme the components to your needs
2. Build your components from scratch using a helper library
3. Build your components from scratch using vanilla web components

### Use an existing library as a base

If you feel that writing web components is too complex, you can start by building on top of an established component library. Libraries like [Lion](https://lion-web.netlify.app/) and [Shoelace](https://shoelace.style/) offer fully-functioning components but with enough styling hooks to let you style them to your needs.

This is a great way for developers new to web components to become more familiar with them. At the same time, you’re offsetting a lot of code complexity to a battle-tested library. Choosing the right library means you’ll have robust, accessible components out of the box. This is a great option if you want drop-in components, that are highly customisable while offering a great user experience for your end users.

### Building components using a helper library

What if you want to go deeper into web components and a quick development cycle?

Or what if you want even more control over how your components behave?

You can use a library to make writing web components more ergonomic. These tools don’t offer any drop-in components, but instead abstraction on top of the vanilla web component API. Some of the most popular tools are:

- [Lit](https://github.com/lit/lit/) - _a simple library for building fast, lightweight web components._
- [Stencil](https://stenciljs.com/) - _a simple compiler for generating Web Components and static site-generated PWAs_
- [Atomico](https://github.com/atomicojs/atomico/) - a _micro-library for creating web components using only functions, hooks and virtual-dom._

I’ve used Lit the most and it’s my go-to when building web components from scratch. It has great TypeScript support, a stripped-back API, and a strong philosophy of leaning on the browser standards.

Stencil is another solid option. It provides a lean API and framework-like tooling, like a compiler to turn its specific flavour of web components into browser-friendly code.

Atomico is a good option if you have a React background as it uses JSX-like syntax + React-style hooks.

Some, but not all, of these options require a compiler to make your components browser-ready. This might not be a problem for you, but it does mean there’s an intermediary step between writing your components and using them in the browser.

### Building vanilla web components

When writing web components, there's also the option to go vanilla. Going vanilla is valid if you're prepared to write more verbose code and get acquainted with the [web component specs](https://github.com/WICG/webcomponents).

You’ll learn concepts like the shadow DOM, custom elements, and HTML templates in depth. As a result, it’ll help you better understand and better appreciate some of the previously mentioned libraries. Secondly, you’ll improve your understanding of other browser concepts too, so using web components will make you a more well-rounded web developer.

### Opinion Time (🪙🪙)

Depending on your priorities I’d recommend different approaches.

If you want to build something quickly that’s likely to be used beyond a few users, try building on top of an existing library. A great library will have the following benefits:

- Battle-tested components that have been improved over time
- Accessibility front of mind
- Strong documentation
- It’ll help you develop your interfaces much more quickly

If you want more hands-on coding, then I’d recommend picking one of the helper libraries: Lit, Atomico, or Stencil. I personally lean toward Lit, I’ve used it in heaps of projects. I also use Lit because it doesn’t require a compiler to make it browser-compatible.

Finally, if you want to get stuck into the lower-level APIs of web components, then I’d recommend exploring building your own web components. It’s a great way to better understand the browser platform, and helps you become a more well-rounded developer.

The documentation for web components can be super fragmented, but [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_components) is a great spot to go if you want to get started. You can also check out [Component Odyssey](https://component-odyssey.com/), which teaches you the fundamentals of web components in a fun, and practical course.

## Should I export my library as a single package or as scoped packages?

Many component libraries offer access to the entire component suite via a single NPM package, while others split components out into their own packages. Let’s take a look at both approaches.

### Single Package

This is where your consumers install your component library via a single package.

Libraries like [Material Web](https://material-web.dev/) offer a core UI module that exports a few dozen components. You'd install via:
`npm install @material/web`
and use the components like so:

```html
<script type="module">
  import "@material/web/button/filled-button.js";
  import "@material/web/switch/switch.js";
</script>

<md-filled-button>Click Me</md-filled-button>
<md-switch></md-switch>
```

### **Scoped Packages**

This is where your users install a package for each component they want to use. Libraries like [Radix UI](https://www.radix-ui.com/) take the “scoped package” approach. With this approach, each component in the library can be installed independently.

> The following is an example using a React component library, since I can’t find an example of a well-used web component library that takes this approach.
> If you want to use [Radix UI’s](https://www.radix-ui.com/) checkbox component, you'll need to install a dedicated checkbox package:
> `npm install @radix-ui/react-checkbox`
> and use the components like so:

```jsx
import * as Checkbox from "@radix-ui/react-checkbox";

const CustomCheckbox = () => (
  <Checkbox.Root>
    <Checkbox.Indicator>
      <CheckIcon />
    </Checkbox.Indicator>
  </Checkbox.Root>
);

export default CheckboxDemo;
```

You would then need to install an additional dependency if you wanted to install other Radix components, like their`Slider` or `Popover` components.

### **Which choice should I choose?**

When deciding, you consider the following questions:

- Does your library incorporate an opinionated design language or encourage the use of several components?
- Do you want to avoid the complexity of creating and maintaining multiple packages?
- Do you expect your end-users to use only a handful of components?\*
- Do you want the freedom to version components separately? (we'll get to this later)
  If you answered yes to the first two questions, you'll want to consider exporting your components via a single package.
  If you answered yes to the last two questions, you'll want to consider scoping your packages. There’s a bit of nuance with the third answer, which I talk about below.

### **Does your library incorporate a design language or encourage the use of several components?**

Many component libraries are opinionated about how you design and build your UIs. They may share a common design language, or a consistent API design across components. Chances are, you’re not mixing and matching between these component libraries in a single project. In this case, it makes sense to export a component as a single package.

Other projects like Radix, offer fully-functional, white-labelled components that a consumer can pick and choose from. Consumers can pick components without being bound to that library’s visual design language. In cases like this, a consumers may only need to use a small handful of components. If you’re going down this approach, then publishing each component as a separate package is a viable option.

### Do you expect your end-users to use only a handful of components?

Not everyone who uses a component library uses every component that the library offers.

You might be concerned that by offering a single package, your consumers’s application bundles will contain any unused components. If this happens, tons of unused code will be sent to their end users.

Back before the ES Module system was widely supported in the browser, this was a problem, as it was difficult for bundlers to remove unused code. Nowadays, ES Modules are widely-supported, and since it allows bundlers to perform _static analysis_ of your imports, bundlers can more easily remove unused from the consumers bundle.

If you’d like to learn more, [Component Odyssey](https://component-odyssey.com/) goes deeper into the browser’s module systems.

### Opinion Time (🪙🪙)

You might think that the _single package_ approach is only good for smaller, “simple” component libraries, but there are some pretty packed-out libraries that use this approach. One such example is [Shoelace](https://shoelace.style/).

I’ve played around with both solutions in the past and have found that the complexity of managing multiple packages is often unnecessary for a component library (but totally valid for other use cases!).

It’s also important to think about your end users. Installing one package is easier than multiple packages. The package update experience is more cumbersome when each component is managed by a separate package. I discuss versioning further down though.
My general philosophy is to opt for the simplest approach and only add complexity when necessary.

## How should I structure the repo of my library?

If you’re publishing a single package, you’ll need to create a top-level package.json file that encompasses every component in your library. A standard of a component library may be structured in the following way:

```yaml
component-library/
src/
button/
button.js
checkbox/
checkbox.js
package.json
```

If you’re exporting scoped packages, each package will be published independently. This means that each component will need a `package.json` file. Since each component has its own `package.json` file, it will have a degree of autonomy that it wouldn’t have otherwise. It can have its own scripts, dependencies, and versions, etc. This approach, where your component library is comprised of distinct projects, is known as a monorepo. This is what your folder directory might look like:

```yaml
component-library/
packages/
button/
button.js
package.json
checkbox/
button.js
package.json
package.json
```

In my experience, the key benefits of using monorepos for UI libraries are:

- publishing components independently
- scoping builds and scripts, which can speed things up in particularly large libraries
- logically group batches of components

For that last point, you might have a track of alpha/experimental components that exist outside the core component library. Using a monorepo would allow you to develop and publish these experimental components at a different cadence to the rest of the your components.

All the major package managers have support for monorepos, so you won’t need to setup and install a brand new tool if you want to explore them.

[npm](https://docs.npmjs.com/cli/v10/using-npm/workspaces) | [yarn](https://yarnpkg.com/features/workspaces) | [pnpm](https://pnpm.io/workspaces)

There are dedicated tools that offer a more robust monorepo experience though, and these can come in handy if you’re dealing with a particularly large or complex repo. [Turborepo](https://turbo.build/repo/docs), for instance, requires additional configuration but comes with the benefits of caching builds etc.

### Opinion Time (🪙🪙)

I’ve worked in large backend/frontend monorepos, I’ve also built component libraries using monorepos, like [A2K](https://a2000-docs.netlify.app/). Unless you’re really set on versioning your component library, I’d advise on avoiding using monorepos, at least to start with.

## Should I use tooling to build and/or bundle my library?

One perk of using native browser tech for your components is that it can simplify your development environment. If you’re building vanilla web components or a library like Lit, you won’t need a build tool or a compiler to transpile your code into something browser compatible.

With that said, there may be cases where you’ll want to use a build tool. Maybe you want some extra language features, or you want to use a tool like TypeScript. Maybe the tool you’re using to write web components requires a build step. Stencil has a compiler, while Atomico offers two ways of building components, one that’s web-compatible, and another that requires a build step.

When it comes to building your library, three popular options are:

- No build (going buildless)
- Compiler, like TypeScript and Stencil
- Configuring a build tool

### Buildless

Heaps of tools we use to build user interfaces are not natively supported in the browser. For some tools, this is obvious, like a Python templating engine. Others less so. Libraries like React require a build tool to run in the browser, to transform JSX into something browser-friendly.

Web components are native to the browser, meaning you won’t have to run a build to get them working in the browser. You can take this philosophy and use it when developing your component library altogether. You might want to avoid ANY tool that requires a build tool. This kind of workflow, where the code you write is browser compatible is called “buildless”, i.e., no build step is required to run it.

This kind of workflow is becoming more popular, there have been a few [high-profile](https://world.hey.com/dhh/you-can-t-get-faster-than-no-build-7a44131c) [instances](https://devclass.com/2023/05/11/typescript-is-not-worth-it-for-developing-libraries-says-svelte-author-as-team-switches-to-javascript-and-jsdoc/) of teams removing reliance on building tooling. My first exposure to a buildless workflow was through [Pascal Schilp's](https://css-tricks.com/going-buildless/) 2019 article, which demonstrates how much one could achieve going buildless.

### **Compilers (TypeScript, Stencil)**

You might want to use a language or a tool that’s fundamentally incompatible with the browser. Tools like TypeScript are incredibly popular as they introduce features on top of JavaScript, like types, and a compiler to catch bugs before they reach your user.

Adding a compiler requires a compilation step before you develop and publish your library. If you were to publish TypeScript code, then your consumers will need to setup TypeScript to use your library.

The same goes for Stencil, and other tools like it. The main problem with introducing these kinds of tools is that you add an intermediary step before running and publishing your code. This means that your compiler is a single point of failure, if there’s a bug or a “skill issue” then it can slow down productivity. You also risk making it more challenging for people to contribute to your library as they require learning the tool to contribute.

That isn’t to say that these tools are bad. They may offer benefits user experience or developer experience benefits that may be more difficult (or impossible) to achieve without such tooling.

### Build Tools (Vite)

During development, you’ll need to run a development server. Many build tools double up as development servers, but some subtly inject non-standard behaviour into your library.
If you were running a Vite server, the following JavaScript would work out of the box:

```jsx
import './index.css';

export function main() {...}
```

Vite automatically adds non-standard browser behaviour to your project. If you were to publish this code without transforming, then it can cause problems for consumers who don’t use the same tool as you.

Not all tools are like this though, [Web Dev Server](https://modern-web.dev/docs/dev-server/overview/) is a development server that doesn’t implicitly opt you into browser incompatible behaviour. I use this a lot when building web components.

### Opinion Time (🪙🪙)

When it comes to build tooling, my preferences depends on my project. If I’m building an application, I’ll reach for TypeScript. If I’m building a component library, I’ll avoid TypeScript. I’ve been using [JSDoc](https://jsdoc.app/) more recently, which offers browser friendly type safety via code comments.

## Aside: Application-level concerns vs library-level concerns

You may be tempted to perform certain optimisations like minifying, bundling, and transpiling your code before publishing. You might want to reconsider, should a library developer be concerned with these optimizations? My answer is no.

For starters, you don’t know the application requirements for every application that uses your library. They could support modern browsers only, they may support IE11, they might have a bandwidth budget, they might not. It’s the application developer’s responsibility to ensure their application satisfies their user’s needs. It might be that they need to minify their code, bundle it into a single output, and transpile it down to an older version of JavaScript.

Let’s take a look at bundling, which is an application-level concern. Bundling is the process of combining several modules into fewer, but larger chunks of code. The developer should find the right balance to ensure that the end-user has all the code they need in a timely manner. It’s not the responsibility of the library author to perform this optimisation as the needs of applications can vary drastically. Premature optimisations can even be actively hostile towards developers consuming your library. In the past, I used a library that minified their published code making it very difficult to debug issues.

As a library author, you can export your component as modern, browser-friendly JavaScript using ESM, and allow your consumers to do whatever they need to with your code. I cover this approach in [Component Odyssey](https://component-odyssey.com/).

## How can I safely publish my library?

The way you version and publish your library depends on whether you choose to structure your repo as a monorepo.

Versioning and publishing multiple packages within a monorepo is more complex than simply publishing a single package.

Let’s take a start by looking at the latter:

### Publishing a single package

The quickest way to publish your package is to run `npm publish`, but I’d advise against doing so. The default publishing script has few safeguards to prevent you from making common publishing mistakes.

It’s easy to:

- Publish uncommitted changes
- Publish from the wrong branch
- Publish with failing broken tests

A tool like [NP](https://www.npmjs.com/package/np) is more appropriate. It describes itself as “A better `npm publish`" because it protects the developer from making the above mistakes.

### Publishing packages within a monorepo

If publishing multiple packages, you’ll also need to decide whether you want all of your packages to be _fixed_ to the same version or to be _independently_ versioned.

### **Fixed**

In this scenario, all the packages in the monorepo share the same version. If your checkbox component has a minor change and gets bumped to 1.2.0, your alert component will also get bumped to version 1.2.0.

The downside is that new versions of a component may be published even if it hasn’t changed. This can lead to a confusing and tedious upgrade experience if a developer uses many packages from your component library.

### **Independent**

In this scenario, all the packages across the monorepo have independent versions. If your checkbox component has a minor change and gets bumped to 1.2.0, your alert component's version won't change.

This is a good approach if you expect users to use a handful of your components, or if the components are discrete in their utility. It’s also good for separating components that change more frequently, like alpha or experimental components.

Managing independent versions is a tricky operation, especially if some components are dependent on others. The popular package managers don’t have great support for this workflow either, so you’ll likely need to opt for a dedicated publishing tool.

[Changesets](https://github.com/changesets/changesets?tab=readme-ov-file) is a great tool to de-risk and automate the publish process in a monorepo. You describe your changes to Changesets as you develop. When you’re ready to publish, Changesets aggregates these files, determines the correct version for each changes package, and publishes to NPM.

### Opinion Time (🪙🪙)

I’ve published both single-package component libraries and multiple-package monorepos.

If the component library is cohesive in terms of its utility or appearance I would publish a single package. It’s easier to version, and easier for developers to consume.

If you want to publish multiple components, I’d suggest managing independent versions. I’ve been on the other end of upgrading multiple packages from an organisation that adheres to fixed versioning and it’s tedious.

## What should my library’s testing strategy be?

The first [known software testing team was created in the 1950s](https://www.testingreferences.com/testinghistory.php), so I can only guess that debates about testing software predate any debates on web frameworks by several decades.

Even now, you’re likely to run into folks with a different preference for software testing. Some people swear by unit tests, others only prefer end-to-end testing. This is often visualised as a “testing pyramid” a “testing trophy”, or a different shape entirely. Whatever testing polygon you devote your practice to, it’s important to establish a testing strategy for multiple reasons:

1. You reduce the risk of publishing a broken library
2. You can test within the boundaries of your library’s intended usage
3. You can establish a precedence for other contributors to follow

Testing in the browser presents a unique set of challenges not present in other forms of software. For starters, you need to support multiple browsers. Each browser also has multiple versions and so browser APIs will be supported to varying degrees across browsers and their versions.

It’s also important to think about your users too. Since your component library will contain heaps of user interface controls, they should behave as users expect.

Let’s look at different types of testing you can employ within a testing strategy.

### Headless browser testing

This approach involves testing your web components in a browser without a UI. Your test runner will spin up a browser in a terminal, load your component, and run your test cases.

The benefits of this approach are:

- Tests are quick, due to no browser UI overhead
- You test using an actual browser, and not a simulated environment
- Your tests focus on behaviour, not the component’s implementation

I particularly like this quote from Kent C Dodds, creator of the Testing Library family of browser testing tools:

> The more your tests resemble the way your software is used, the more confidence they can give you.

[Playwright](https://playwright.dev/) is a browser testing tool that lets you run your code in Chrome, Firefox, and Safari. Playwright is a great all-purpose tool, but if you specifically want to test your web components, then you can use [Web Test Runner](https://modern-web.dev/guides/test-runner/getting-started/), which combines a browser launcher, like Playwright with a DX focused on component testing.

You can take this even further and write tests that ensure your components work across different frameworks too. The [PatternFly Elements](https://github.com/patternfly/patternfly-elements) repo also tests across different web frameworks.

### Visual Regression Testing

It’s easy to miss subtle styling regressions if you’ve made significant changes to a component.

- [ ] Do a comparison of the two

Eagle-eyed developers will this problem, but if you’re working in a team, a fast-moving environment, or you’re accepting open-source contributions, then it makes sense to have a programmatic way of detecting changes.

You can use a visual regression tool to take visual snapshots of your component, before and after changes. You can integrate this within your testing pipeline to ensure any snapshots with visual changes are manually reviewed.

The benefits of this approach are:

- Isolating and spotting small UI problems that could slip through the cracks
- Aligning different stakeholders and getting sign-off.

In traditional teams, design and development may be two separate business departments. A designer may be responsible for the visual design, and the developer may be responsible for the implementation. Snapshot tests are a great way of involving the designer in the QA process.

Snapshot testing tools aren’t as straightforward to set up, they often require:

- Storage for snapshot images
- Integrations with tools like GitHub Actions to pass/fail the pipeline
- A microsite to display the snapshot and accept user actions

Some handy snapshot testing tools out there are:

- [Percy](https://percy.io/) (paid)
- [Scooby](https://github.com/AnimaApp/scooby) (open source)

### Manual testing

A complete automated testing suite means you can spend more time performing exploratory testing to catch issues that automated tests can't find.

Here’s an excellent article about integrating [accessibility testing into your development process](https://www.smashingmagazine.com/2021/04/bake-layers-accessibility-testing-process/). The article recommends a handful of ways of manually testing that your UI is accessible, which I've adjusted slightly to focus on testing components:

- Can you use your components without your mouse? Use simple keyboard-only manual testing to evaluate new components.
- Can you still use your components when setting browser magnification to 200% or greater?
- Do your components have a dark mode? Is this dark mode suitable for those with light sensitivity?
- Perform testing using assistive technology (VoiceOver, Microsoft Narrator, and NVDA are free options).

If you're interested in your UI library being accessible, [and you really should be](https://www.w3.org/WAI/fundamentals/accessibility-intro/#important), then make time for manual accessibility testing. Making your components usable for those with sight, hearing, cognitive, or motor disabilities will also make your components more usable for [everyone else](https://www.w3.org/WAI/perspective-videos/keyboard/).

If you’re interested in writing more accessible components here are a few great resources:

- [Carie Fisher’s A11y Style Guide](https://a11y-style-guide.com/style-guide/)
- [Heydon Pickering’s Inclusive Components](https://inclusive-components.design/)
- [ARIA Authoring Practices Guidelines](https://www.w3.org/WAI/ARIA/apg/patterns/)

### Static Analysis Testing

Finally, there are _static analysis testing_ tools like linters, e.g., [ESLint](https://eslint.org/). These tools review your code and flag issues in your editor as you type, some even automatically fix these problems. The ESLint ecosystem has plugins that can spot issues for web components. [ESLint Plugin Lit](https://www.npmjs.com/package/eslint-plugin-lit) is one such example.

These tests run in your editor as you work, so the overhead for adding and running ESLint is small. You can run linters, at any stage of your development lifecycle; before committing, before pushing, when opening a pull request, before publishing, etc.

Linters aren’t the only static analysis tools, TypeScript is another example. Like ESLint, it has IDE integration, so your editor can spot problems in your code before you compile it.

## How to write documentation for my component library

Creating clear and easy-to-use documentation is one way to make consumers fall in love with your library. You’ll also want to create documentation that integrates nicely within your workflow. Here are some different ways you can provide component documentation to your users.

### Option 1: README.md

If your codebase is publicly displayed on GitHub, you can write your documentation in README files within your repo. [Changesets](https://github.com/atlassian/changesets) does this by using the top-level README as a table of contents, and linking to other READMEs in the repo.

The benefits of this approach are two-fold:

- You don’t need to create and deploy a documentation site
- The markdown files you write now can be used as inputs for static site generators that you might use later on.

You can start simple, and repurpose the markdown you write to generate your documentation site's pages in the future.

This approach doesn’t lend itself well to something as visual and interactive as a component library, where a user may want to play with components themselves.

### Option 2: Markdown-driven documentation tools

If you've written your documentation via option 1, you can use those files as inputs to static site generators like [Starlight](https://starlight.astro.build/) and [Docusaurus](https://docusaurus.io/).

These tools take your markdown and JavaScript and compile it to a static site optimised for documentation. This is handy, but without additional tooling, you’ll need to update your documentation site every time you change your components.

The [Custom Elements Manifest](https://github.com/webcomponents/custom-elements-manifest) (CEM) is a spec that describes how to represent a web component as a JSON object. There’s a [CEM analyser](https://github.com/open-wc/custom-elements-manifest?tab=readme-ov-file) that generates this object for your components. The output can be used to create a host of different tooling around web components. For instance, the [API Viewer](https://api-viewer.open-wc.org/) element is one such tool.

### Option 3: UI Cataloging tools

Beyond static site generators, there are more involved cataloguing tools, like Storybook. These cataloguing tools showcase the full breadth of functionality of your components

[Storybook](https://storybook.js.org/) has a strong ecosystem of plugins that allow developers to simulate events, change viewport width, perform basic accessibility tests, and more.

This approach is fine, but I’ve often seen it used in conjunction with a documentation site, not to replace it.

### Opinion Time (🪙🪙)

In the past, I would use a tool like Storybook. I like how feature-rich it is, and how great the ecosystem is. More recently, I’ve been interested in forgoing a tool like Storybook, and instead of splitting up documentation from component cataloging, provide a single resource for consumers. Other libraries take this approach too. [Shoelace](https://shoelace.style/resources/contributing/#dev-sandbox) discusses why it doesn’t use Storybook in their documentation site.

For a recent project, I created a simple HTML site that consumed my components. it was a quick and easy way of publishing a site and requires little maintenance. The downside is that it’s a very static approach, and won’t scale for a component library that’s quickly growing and changing

I have been exploring using the _custom element manifest_ to automatically generate documentation for my components, but I haven’t cracked an ideal workflow yet.

As for deploying your documentation site, there are heaps of different tools out there to quickly get a static site live. My go to hosting platform for deploying a static site is Netlify. But you can use one of any other hosting site out there.

## What’s next?

There are still heaps more decisions that you need to make when building a component library.

If I’ve missed any, open an issue or a pull request to add it to this document.

If you’re interesting in learning more about building component libraries, then check out my course [Component Odyssey](https://component-odyssey.com/). The course covers web components, licenses, module systems, peer dependencies, and styling solutions.

You’ll learn to save yourself and your company weeks of development time, build components your users will love, and become a future-proof web developer
