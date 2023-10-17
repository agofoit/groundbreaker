# Prepare the frontend app

We already forked our vue+vite app and cleaned it up so that we can focus on setting up our own small example.

Missing in that tutorial: Install and import of CORS and dotenv to the express app.

## What is Vite?

Vite is a build tool and development server designed for modern web development. It was created by Evan You, the same individual who created Vue.js, and it is often used in conjunction with Vue.js, although it can be used for other JavaScript frameworks and libraries as well.


## PART 1: More set up preparations

1. If not done yet, create a `.env` file and add it to the `.gitignore`.
2. Furthermore look for the `vite.config.js` file on root level. Replace its complete content by:

```js
// File: ./vite.config.js

import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig(({ command, mode }) => {
  const env = loadEnv(mode, process.cwd(), '')
  return {
    define: { },
    plugins: [vue()],
    server: { port: env.VITE_PORT }
  }
})
```

3. Our `.env` file should look like this:

```
VITE_PORT=4545
VITE_API_PORT=3001
VITE_SERVER=http://localhost
```

Compare the changes. The changes make it possible to use environment variables in out project and within the vite.config like we do for setting a specific server port that we can define and overwrite in our `.env`.


## PART 2: Improve the HelloWorld.vue component

1. Let's import everything we need: We will import `onMounted` function to hook in the lifecyle of the vue component (called Lifecycle Hooks). Here we can run code before the component gets mounted. Furthermore we import `ref` to get some reactivity functionality to the app. We also use `axios` here to send requests to the server. Currently Vue has also a built in `fetch` functionality that also could have been used here.
  We also declare our server and port in this place. It is important to prefix the environment variables with `VITE_` !
  Also let's declare our variable "helloString" as shown in the example.

```js
// File: ./src/components/HelloWorld.vue

<script setup>
  import { onMounted, ref } from 'vue'
  import axios from 'axios'
  const server = import.meta.env.VITE_SERVER
  const port = import.meta.env.VITE_API_PORT ? ':' + import.meta.env.VITE_API_PORT : ''

  const helloString = ref('...')
</script>
```

2. Now we can also update our template so that it looks like in the following code snippet to render our helloString in our web app in the browser:

```js
// File: ./src/components/HelloWorld.vue

<template>
  <h1>{{ helloString }}</h1>
</template>
```

3. Between the `<script>` tags we will now add our onMounted functionality.

```js
// File: ./src/components/HelloWorld.vue

onMounted(async () => {
  const {data} = await axios.get(`${server}${port}/api/hello`)
  helloString.value = data.helloString
})
```

As you can see we call the servers endpoint "/api/hello" that doesn't exist in our express app yet. We will adjust it soon!


## PART 3: Adjust our api endpoint on server side

1. In our express app we can add a new endpoint to the `app.js`

```js
// File: ./app.js

app.get('/api/hello', async (req, res) => {
  const helloString = await getHelloData()
  console.log(new Date(), helloString)
  res.json({ helloString });
})
```


## PART 4: Let's start our app!

1. Start your express app or "server" with the command `npm run start`. Check if it runs without any errors. It should run on port 3001.
2. Start your vue app or "frontend" with the command `npm run start`. Check if it runs without any errors. It should run on port 4545.
3. Open `http://localhost:4545/` in your browser. Does it display "Hey"?


## PART 5: Add more strings to the database

1. Login to your mongoDB Atlas account, visit your collection and insert at least two more "documents".
2. Return to `http://localhost:4545/` and refresh the page multiple times. Does the displayed string change?

---

In the next steps we can provide an input element in the web app that sends new helloStrings to the server and the server to the database! If you finished that tutorial I can provide a third one that shows you how to write in the database from your Vue app!

Let me know if those parts worked out or if you get stuck somewhere. I am always happy to help, just write me on Slack!