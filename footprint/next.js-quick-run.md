## Next.js quick run

### install
+ `npm install next react react-dom`

### create app
+ npx create-next-app
+ or
+ yarn create next-app
+ It seems default using is yarn.
+ so you can type `yarn dev`

### conceptions
+ `pages` directory is the core. All the `.js or .jsx, .tx .tsx` and react components are in here.
+ `pages/about.js` is mapping to `/about`.
+ `pages/posts/[id].js` can be accessed with `posts/1`,`posts/2`.
+ `./public/` is mapping to `/`, mostly are static files.
+ Next.js will build and generate the static html files mostly.
#### dynamic content in pages
+ Get content from outside: `getStaticProps`
+ 
```
// TODO: Need to fetch `posts` (by calling some API endpoint)
//       before this page can be pre-rendered.
function Blog({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li>{post.title}</li>
      ))}
    </ul>
  )
}

export default Blog
```
+ 
```
function Blog({ posts }) {
  // Render posts...
}

// This function gets called at build time
export async function getStaticProps() {
  // Call an external API endpoint to get posts
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // By returning { props: posts }, the Blog component
  // will receive `posts` as a prop at build time
  return {
    props: {
      posts,
    },
  }
}

export default Blog
```

+ Get dynamic routes and contents from outside: `getStaticPaths`,`getStaticProps`.
+ 
```
// This function gets called at build time
export async function getStaticPaths() {
  // Call an external API endpoint to get posts
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // Get the paths we want to pre-render based on posts
  const paths = posts.map((post) => `/posts/${post.id}`)

  // We'll pre-render only these paths at build time.
  // { fallback: false } means other routes should 404.
  return { paths, fallback: false }
}
```
+ 
```
function Post({ post }) {
  // Render post...
}

export async function getStaticPaths() {
  // ...
}

// This also gets called at build time
export async function getStaticProps({ params }) {
  // params contains the post `id`.
  // If the route is like /posts/1, then params.id is 1
  const res = await fetch(`https://.../posts/${params.id}`)
  const post = await res.json()

  // Pass post data to the page via props
  return { props: { post } }
}

export default Post
```
+ More...[pages](https://nextjs-cn.com/docs/basic-features/pages),[data](https://nextjs-cn.com/docs/basic-features/data-fetching#fetching-data-on-the-client-side)
+ [Demos](https://github.com/vercel/next-learn/tree/master/basics).