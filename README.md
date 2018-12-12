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

<hr>

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
<hr>

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

<hr>

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
<hr>
First of all, let's add the list of post titles in the home page.
<br>
Add the following content to the pages/index.js.

```jsx
import Layout from '../components/MyLayout.js'
import Link from 'next/link'

const PostLink = (props) => (
  <li>
    <Link href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
)

export default () => (
  <Layout>
    <h1>My Blog</h1>
    <ul>
      <PostLink title="Hello Next.js"/>
      <PostLink title="Learn Next.js is awesome"/>
      <PostLink title="Deploy apps with Zeit"/>
    </ul>
  </Layout>
)
```

Once you add this content, you will see a page like this:
![add dynamin page](https://cloud.githubusercontent.com/assets/50838/24542722/600b9ce8-161a-11e7-9f1d-7ed08ff394fd.png)

Next, click the first link. You'll get a 404 page. That's fine.
<br>
What's the URL of that page?

> /post?title=Hello%20Next.js

We are passing data via a query string parameter (a query param). In our case, it's the “title” query param. We do this with our PostLink component as shown below:

##Create the “post” page
Create a file called `pages/post.js` and add the following content:

```jsx
import {withRouter} from 'next/router'
import Layout from '../components/MyLayout.js'

const Content = (props) => (
  <div>
    <h1>{props.router.query.title}</h1>
    <p>This is the blog post content.</p>
  </div>
)

const Page = withRouter((props) => (
    <Layout>
       <Content />
    </Layout>
))

export default Page
```
Now our page will look like this:
![add dynamin page](https://cloud.githubusercontent.com/assets/50838/24542721/5fdd9c26-161a-11e7-9b10-296d4cb6912d.png)

Here's what's happening in the above code.
- We import and use the "withRouter" function from "next/router" this will inject the Next.js router as a property
- In this case, we are using the router's “query” object, which has the query string params.
- Therefore, we get the title with `props.router.query.title`.

What'll happen when you navigate to this page? [http://localhost:3000/post?title=Hello%20Next.js](http://localhost:3000/post?title=Hello%20Next.js)

> It throws an error

<hr>

##Clean URLs with Route Masking
Here, we are going to use a unique feature of Next.js called route masking. Basically, it will show a different URL on the browser than the actual URL that your app sees.
<br>
Let's add a route mask to our blog post URL.
<br>
Use the following code for the `pages/index.js`:

```jsx
import Layout from '../components/MyLayout.js'
import Link from 'next/link'

const PostLink = (props) => (
  <li>
    <Link as={`/p/${props.id}`} href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
)

export default () => (
  <Layout>
    <h1>My Blog</h1>
    <ul>
      <PostLink id="hello-nextjs" title="Hello Next.js"/>
      <PostLink id="learn-nextjs" title="Learn Next.js is awesome"/>
      <PostLink id="deploy-nextjs" title="Deploy apps with Zeit"/>
    </ul>
  </Layout>
)
```

In the `<Link>` element, we have used another prop called “as”. That's the URL which we need to show on the browser. The URL your app sees is mentioned in the “href” prop.
<hr>

###Reload
Now go to the home page: [http://localhost:3000](http://localhost:3000)
<br>
Then click on the first post title. You'll be navigated to the post page.
<br>
Then reload the browser. What will happen?

> It will throw a 404 error. 

It gives us a 404 error. That's because there is no such page to load on the server.
<br>
The server will try to load the page `p/hello-nextjs`, but we only have two pages: `index.js` and `post.js`.