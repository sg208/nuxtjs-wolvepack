# nuxtjs-wolvepack
Stacks
- NuxtJS (with vue@2.6.14)
- Javascript
- BootstrapVue (SCSS)
- Vuex (currently utilizing state/mapState)

DevOps Cloud Platform (CI/CD)
- Cloudflare Pages (first-three-hours)
- Render.com (production & development)

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
# create .env file at the root dir to store all secret keys to be able to render locally. They are currently set as environment variables to services at runtime, as well as during builds and deploys. They keys can be found on select components using Google Chrome Dev Tools while inspecting the network activity, in either production or development environments. Best hint I can give without revealing the actual keys publicly :)
The content of .env file should be as follow:

API_SECRET_KEY=**EnterOnlyTheUniqueKeyHereWithoutBearer**
GOOGLE_MAP_STATIC_API_KEY=**EnterUniqueApiHere**
GOOGLE_MAP_EMBED_API_KEY=**EnterUniqueApiHere**

# install dependencies
$ npm install

# serve with hot reload at localhost:8000
$ npm run dev
```
