# nuxtjs-wolvepack
Stacks
- NuxtJS (with vue@2.6.14)
- Javascript
- BootstrapVue (SCSS)
- Vuex (currently state/mapState only)

DevOps Cloud Platform (CI/CD)
- Render.com (production & development)
- Cloudflare Pages (first failover for production & development)
- Vercel.com (second failover for production & development)

## Environments

The first three hours of development
* Preview URL: https://b72d070f.nuxtjs-wolvepack.pages.dev/
* Branch: [first-three-hours](https://github.com/sg208/nuxtjs-wolvepack/tree/first-three-hours)

Production (latest features and fixes)
* Preview URL: https://wolfpack.engg.me
* Branch: Master

Development (future fixes and enhancements)
* Preview URL: https://dev-wolfpack.engg.me
* Branch: [development](https://github.com/sg208/nuxtjs-wolvepack/tree/development)

## Build Setup

```bash
# Create .env file at the root dir to store all api keys to
# render locally. They are set as environment variables to
# services at runtime, builds and deploys. The keys can be found
# on select components using Chrome DevTools while inspecting
# the network activity, in production or development environments.
# Best hint I can share without revealing the actual keys publicly.
# The content of .env file should be as follow.

API_SECRET_KEY=**EnterOnlyTheUniqueKeyHereWithoutBearer**
GOOGLE_MAP_STATIC_API_KEY=**EnterUniqueApiHere**
GOOGLE_MAP_EMBED_API_KEY=**EnterUniqueApiHere**

# Install dependencies
$ npm install

# Serve with hot reload at localhost:8000
$ npm run dev
```
