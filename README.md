# next-env-prod-example

Example code for configuring environment variables in Next.js for dev and production on [Zeit Now](https://zeit.co/home).

---

With its hybrid server- and client-side rendering, Next.js can make accessing environment variables a bit nuanced. This example should help you to ensure your environment variables are available on both client and server in both dev and production. 

## Setup

### **.env** 
Private tokens and secrets should be stored in a .env file at the root of your app's directory:

```
DEMO_KEY=abc123
```

These variables will be available to your app's server-side code (such as routing in ./pages/api/...) however we still need to expose them to the client side. 

### **next.config.js**
To expose the env variables to the client side, you must add a file named next.config.js to your app's root directory in the format below. Next will use this to expose your desired env variables to the client at build time.  

```javascript
require('dotenv').config()

module.exports = {
  env: {
    DEMO_KEY: process.env.DEMO_KEY
  },
}

```

### now.json
To provide the env variables in production using Zeit Now, first create a now.json configuration file in your app's root directory with the below format. Now uses a `"@<KEY_NAME>"` format to reference the actual value of the env variable which we will add through the Now CLI in the next step. 

```javascript
{
  "env": {
    "DEMO_KEY": "@demo_key"
  },

  "build": {
    "env": {
      "DEMO_KEY": "@demo_key"
    }
  }
}
```

### Now CLI
Install Zeit Now:

```
$ npm i -g now
```

Define new secret(s) that corresponds to the reference(s) in now.json:

```
$ now secrets add demo_key abc123
```

Now when you run `$ now` to deploy your app, your env variables will be accessible to your app.

## Things to note
- Always add .env to your gitignore so that your .env is not exposed. I added it to this repo for the sake of completeness
- You can use the separate definition of env variables between dev and prod to define variables unique to each environment, like a baseURL.