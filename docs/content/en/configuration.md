---
title: Configuration
description: 'Easily connect your Nuxt.js application to your content hosted on Prismic'
position: 310
category: 'Module API Reference'
version: 1.2
fullscreen: false
---

You can configure `@nuxtjs/prismic` with the `prismic` property in your `nuxt.config.js` or directly when registering the module in the `buildModules` array by using the array syntax.

<code-group>
  <code-block label="prismic key" active>

```javascript[nuxt.config.js]
export default {
  prismic: {
    /* configuration */
  }
}
```

  </code-block>
  <code-block label="buildModules array">

```javascript[nuxt.config.js]
export default {
  buildModules: {
    ['@nuxtjs/prismic', {
      /* configuration */
    }]
  }
}
```

  </code-block>
</code-group>

## Properties

### endpoint

- Type: `String`
- `required`

The endpoint of your Prismic repository.

```javascript[nuxt.config.js]
prismic: {
  // getting content from the Prismic repository named `my-repo`
  endpoint: 'https://my-repo.cdn.prismic.io/api/v2'
}
```

### apiOptions

- Type: `Object`
- Default: `{}`

Options sent to Prismic API when initing the client, see [Prismic documentation](https://prismic.io/docs/rest-api/basics/introduction-to-the-content-query-api#4_1-the-api-search-endpoint).

```javascript[nuxt.config.js]
prismic: {
  // example querying a private Prismic repository
  // please note that the token will bleed in the front-end
  apiOptions: {
    access_token: 'yourAccessToken'
  }
}
```

#### New Routes Resolver

<alert>

This feature has not been propagated on all clusters, contact us [on the forum](https://community.prismic.io/c/kits-and-dev-languages/vue-js/16) if you want to try it out!

</alert>

A new routes resolver is being introduced, which replaces the [link resolver](#linkresolver) function with a single JSON declaration. You can take advantage of it by passing a `routes` options to the API:

```javascript[nuxt.config.js]
prismic: {
  apiOptions: {
    // example resolving documents with type `page` to `/:uid`
    routes: [
      {
        type: 'page',
        path: '/:uid'
      }
    ]
  }
}
```

Documents coming from the API will then have a new `url` property, removing the need to use a link resolver.

<alert type="info">

Learn more about the new routes resolver [here](https://www.slicemachine.dev/documentation/nuxt/link-resolver).

</alert>

### preview

- Type: `Boolean|String`
- Default: `true`

Activate previews on the website, learn more about the preview feature [here](/previews).

```javascript[nuxt.config.js]
prismic: {
  preview: false // disable previews
}
```

### components

- Type: `Boolean`
- Default: `true`

Registers [@prismicio/vue](/injected-kits#prismiciovue) components globally.

```javascript[nuxt.config.js]
prismic: {
  components: false // disable components global registration
}
```

### linkResolver

- Type: `String(path)|Function`
- Default: `~/app/prismic/link-resolver.js`

Your [link resolver](https://prismic.io/docs/vuejs/beyond-the-api/link-resolving) function. By default the module expects you to have it exported at `~/app/prismic/link-resolver.js`, see [installation](/installation).

```javascript[nuxt.config.js]
prismic: {
  // you can provide your link resolver function directly
  linkResolver: function(doc) {
    return '/'
  }
}
```

### htmlSerializer

- Type: `String(path)|Function`
- Default: `~/app/prismic/html-serializer.js`

Your [html serializer](https://prismic.io/docs/vuejs/beyond-the-api/html-serializer) function if you need one. By default the module expects you to have it exported at `~/app/prismic/html-serializer.js`.

### disableGenerator

- Type: `Boolean`
- Default: `false`

When using the [new routes resolver](#new-routes-resolver) this module is providing a crawler feature that will crawl your Prismic documents and extending Nuxt.js `generate.routes` array (see [Nuxt.js documentation](https://nuxtjs.org/guides/configuration-glossary/configuration-generate#routes)) with routes to those.

Since version 2.13 Nuxt.js is shipped with a [built-in crawler](https://nuxtjs.org/guides/configuration-glossary/configuration-generate#crawler) so this module's crawler feature is being deprecated. If you're using Nuxt.js >= 2.13 you should be interested in disabling this module's generator with this option.

```javascript[nuxt.config.js]
prismic: {
  disableGenerator: true // disable module's crawler
}
```

## Defaults

Default configuration only expects you to provide [your Prismic API endpoint](#endpoint).

```javascript[nuxt.config.js]
export default {
  prismic: {
    preview: true,
    components: true
  }
}
```
