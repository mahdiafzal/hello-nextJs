# nextJs

To start, create a sample project by running the following commands:

```node
mkdir hello-nextJs
npm init -y
npm install --save react react-dom next
mkdir pages
```

Then open the "package.json" in the hello-nextJs directory and add the following NPM script.

```json
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

Create a file named `pages/index.js` and add the following content:

```jsx
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```

Now if you visit [http://localhost:3000](http://localhost:3000) again, you'll see a page with "Hello Next.js"