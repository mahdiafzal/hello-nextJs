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
<br />
create an "About" page by adding the following content to `pages/about.js`:

```jsx
export default () => (
  <div>
    <p>This is the about page</p>
  </div>
)
```

Then we can access that page with [http://localhost:3000](http://localhost:3000).
<br />
After that, we need to connect these pages. We could use an HTML "a" tag for that. However, it won't perform client-side navigation; it navigates to the page via the server, which is not what we want.n order to support client-side navigation, we need to use Next.js's Link API, which is exported via `next/link`.
<br />
Add the following code into `pages/index.js`:

```jsx
// This is the Link API
import Link from 'next/link'

const Index = () => (
  <div>
    <Link href="/about">
      <a>About Page</a>
    </Link>
    <p>Hello Next.js</p>
  </div>
)
```

Here we've imported `next/link` as `Link` and use it like this:

```jsx
<Link href="/about">
  <a>About Page</a>
</Link>
```

Now try to visit [http://localhost:3000](http://localhost:3000)

Then click the "About Page" link. It will navigate you to the "About" page.

> This is client-side navigation; the action takes place in the browser, without making a request to the server.
> You can verify this by opening your browser's network request inspector.

Okay, now we have a simple task for you:

- Visit [http://localhost:3000](http://localhost:3000)
- Then click the "About Page"
- Then hit your browser's Back button

How would you best describe the experience of the Back button?

> It navigated the page to the index (home) page via the client side.

Add the following to the file `components/Header.js`.

```jsx
import Link from 'next/link'

const linkStyle = {
  marginRight: 15
}

const Header = () => (
    <div>
        <Link href="/">
          <a style={linkStyle}>Home</a>
        </Link>
        <Link href="/about">
          <a style={linkStyle}>About</a>
        </Link>
    </div>
)

export default Header
```

Next, let's import this component and use it in our pages. For the `index.js` page, it will look like this:

```jsx
import Header from '../components/Header'

export default () => (
  <div>
    <Header />
    <p>Hello Next.js</p>
  </div>
)
```

You can do the same for the about.js page as well.
<br>
<hr>
In our app, we'll use a common style across various pages. For this purpose, we can create a common Layout component and use it for each of our pages. Here's an example:
<br>
Add this content to `components/MyLayout.js`:

```jsx
import Header from './Header'

const layoutStyle = {
  margin: 20,
  padding: 20,
  border: '1px solid #DDD'
}

const Layout = (props) => (
  <div style={layoutStyle}>
    <Header />
    {props.children}
  </div>
)

export default Layout
```

Once we've done that, we can use this Layout in our pages as follows:

```jsx
// pages/index.js

import Layout from '../components/MyLayout.js'

export default () => (
    <Layout>
       <p>Hello Next.js</p>
    </Layout>
)
```

```jsx
// pages/about.js

import Layout from '../components/MyLayout.js'

export default () => (
    <Layout>
       <p>This is the about page</p>
    </Layout>
)
```
