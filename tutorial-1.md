# Prepare the backend app

## PART 1: Connect to the database

1. In your project create a new directory 'config'
2. In the folder 'config' create a new file 'mongodb.js'
3. Import mongoose and dotenv for this file and set the connection string from the .env file

```js
// File: ./config/mongodb.js

const mongoose = require('mongoose')
require('dotenv').config()
const mongoDB = process.env.DB_CONNECTION_STRING
```

4. Add a main() function to connect to the database:

```js
// File: ./config/mongodb.js

async function main() {
  try {
    await mongoose.connect(mongoDB, { dbName: 'Hello-Express' })
    console.log('Connected to MongoDB')
  } catch (error) {
    console.log('Error connecting to MongoDB', error)
  }
}
```

5. Also add the following code line to catch and log errors if there are any:

```js
// File: ./config/mongodb.js

main().catch((err) => console.error(err))
```

6. Open the file app.js and import your new db config file like this

```js
// File: ./config/mongodb.js

require('./config/mongodb')
```

7. Start the server with `npm run start` and hopefully read "Connected to MongoDB" in the console.log! That means that your connection string was right and you can connect to the database in the cloud.


## PART 2: Prepare a database model we will work with

1. In your project create a new directory 'models'
2. In the directory 'models' create a new file 'hello.js'
3. Import mongoose and get a reference from the constructor mongoose.Shema. This should look like this:

```js
// File: ./models/hello.js

const mongoose = require('mongoose')
const Schema = mongoose.Schema
```

4. Create a new Schema called 'HelloSchema' that has an id and the property 'HelloString'

```js
// File: ./models/hello.js

const HelloSchema = new Schema({
  id: { type: Number },
  HelloString: { type: String },
})
```

5. Finally provide an export of the new mongoose model:

```js
// File: ./models/hello.js

module.exports = mongoose.model('Hello', HelloSchema)
```

6. Just restart your server to check on any errors.


## PART 3: Prepare a service that can read and write to the database

1. In your project create a new directory 'services'
2. In the directory 'services' create a new file 'helloDataService.js'
3. Import your Hello model

```js
// File: ./services/helloDataService.js

const Hello = require('../models/hello')
```

4. Create a getHelloData() function to read data from your database.

```js
// File: ./services/helloDataService.js

async function getHelloData () {
  const hellos = await Hello.find()
  const randomIndex = Math.floor(Math.random() * hellos.length)
  return hellos[randomIndex].HelloString
}
```

This function receives all hello entries of the database (if there are multiple) and returns only one random entry of it.

5. Export the function

```js
// File: ./services/helloDataService.js

module.exports = { getHelloData }
```

6. Just restart your server to check on any errors.


## PART 4: Prepare the endpoint of the API

1. In your app.js import your helloDataService.js

```js
// File: ./app.js

const { getHelloData } = require('./services/helloDataService.js')
```

2. Add the built-in middleware 'json'. It is used to parse JSON data from the request body.

```js
// File: ./app.js

app.use(express.json())
```

3. Change the default route to the following that it calls the getHelloData() function to get data from the database. Also add a console.log.

```js
// File: ./app.js

app.get('/', async (req, res) => {
  const helloString = await getHelloData()
  console.log(helloString)
  res.send('ok')
})
```

4. Restart your server and reload https://localhost:3001 in your browser. Switch back to the terminal in VSCode and check for the "Hey". Did it work?

Next steps could be to display this string in the browser on https://localhost:3001 and also to add some new strings to the database. Let me know if those parts worked out or if you get stuck somewhere. I am happy to help!