# Write Data from the frontend to the database

## 1. Preparation

First check if your app runs without any problems: Start the server, the frontend and check if it's connected to the database. Check the browser log!


## 2. Add a new service function to your express app

1. Add a new function called `function addHelloData (newHelloString)` to your express service file.
2. Within the function declare a new const variable that calls a new instance of the Hello-Schema.
3. The Schema takes an object with a HelloString property as an argument. In our case pass the variable `newHelloString` from the function call to the new schema instance: `new Hello({ HelloString: newHelloString })`
4. Let's save our new value to the database by calling the save() method on our variable.
5. Add the new function to the export.

<details>
  <summary>Show the solution</summary>

  ```js
  // Project: express app
  // File: ./services/helloDataService.js

  function addHelloData (newHelloString) {
    const newHello = new Hello({ HelloString: newHelloString });
    newHello.save();
  }

  module.exports = {
    addHelloData,
    getHelloData
  }
  ```
</details>


## 3. Define the POST endpoint on server side

1. Add the import statement of our new service function to the top of the `app.js` file.
2. Install `body-parser` and add the import statement to the `app.js` as well.
3. Register the new middleware to our app.
4. Add a new POST endpoint to the app.js in your express app. Name the route `/api/hello`.
5. Deconstruct the data from the `req.body` and save it to a new variable `newHelloString` (Hint: Deconstruction works like this: `const { newHelloString } =`
6. For debugging: Add a console log afterwards with the content of our new variable.
7. Now call our new service function `addHelloData` and pass our new variable to it.
8. Finally send an `ok` as a response for out new endpoint to tell the frontend that there was no error during processing the request.

<details>
  <summary>Show the solution</summary>

  ```js
  // Project: express backend app
  // File: ./app.js

  const { getHelloData, addHelloData } = require('./services/helloDataService')
  const bodyParser = require('body-parser')

  app.use(bodyParser.json());

  app.post('/api/hello', async (req, res) => {
    const { newHelloString } = req.body;
    console.log('New string:', newHelloString)
    addHelloData(newHelloString)
    res.send('ok')
  })
  ```
</details>


## 4. Add a new input field to our HelloWorld.vue component

1. Replace your component template by the following code and explain it.

```js
  // Project: vue frontend app
  // File: ./src/components/HelloWorld.vue

  <template>
    <div>
      <h1>{{ helloString }}</h1>
      <input
        type="text"
        id="inputField"
        v-model="newHelloString"
      />
      <button @click="submitForm">Submit</button>
    </div>
  </template>
```

2. Add a new variable as a reactive reference of Vue to the top of the components file: `const newHelloString = ref('')`.
3. Next to the onMounted function within the script section, add a new function `async function submitForm ()`. The function needs to be async.
4. Prepare a try-catch-block within the function.
5. Expand the following solution section and complete the function code. Let's check what's happening there!

<details>
  <summary>Show the solution</summary>

  ```js
    // Project: vue frontend app
    // File: ./src/components/HelloWorld.vue

    async function submitForm () {
      try {
          const response = await axios.post(`${server}${port}/api/hello`, { newHelloString: newHelloString.value });
          console.log('POST request successful:', response.data);
          newHelloString.value = '';
        } catch (error) {
          console.error('Error sending POST request:', error);
        }
    }
  ```
</details>


## 5. Check your app

Finally run your new code and check if it's working!

Now we are ready to update our express service on render and look for a platform to host our frontend for free (e.g. Netlify)!