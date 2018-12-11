# nextJs

To start, create a sample project by running the following commands:

```
mkdir hello-nextJs
npm init -y
npm install --save react react-dom next
mkdir pages
```

Then open the "package.json" in the hello-nextJs directory and add the following NPM script.

```
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```

Now everything is ready. Run the following command to start the dev server:

```
npm run dev
```

Then open [http://localhost:3000](http://localhost:3000) from your favourite browser.<br />
What's the output you see on the screen?

> 404 - This page could not be found
