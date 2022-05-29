## Headless CMS and Nuxt

The essence of this documentation is to explain how you could use platforms like WordPress to manage content that can be displayed on your web app; blogs for example.

**Problem**: We want to display blog posts on our website at [blog.example.com](http://blog.example.com) or [example.com/blogs](http://example.com/blogs).

Often times it's either we don't want to have anything to do with the admin dashboard for managing content, or there are not enough resources or time to say we want to build a blog feature. So an easy way out is to use Content Management Systems like WordPress to manage your content in this case blog posts. WordPress provides a rich, robust Public REST API for its infrastructure and awesome [documentation](https://developer.wordpress.org/rest-api/reference/) to match.

Using WordPress API we can practically perform any HTTP method on any posts on the blog. Interesting? Ohh yes!!! Check out the documentation [https://developer.wordpress.org/rest-api/](https://developer.wordpress.org/rest-api/)

Now as a Frontend Engineer, we have access to APIs, what next? Ohh yea lets break it down

1: **End goal** is to have a blogs and a single blog-details page

2: **Single blog** details must be SEO enriched [meta title, meta description, image, author, read time and more]

## End Goal

- I’d strongly advise we implement SSG, Statically generating the single blog post pages during build time. Thereby serving users HTML ready pages.
    - An advantage is that users won't need to wait for API calls before they can see the content to be read.
    - SEO crawlers can easily crawl through HTML thus giving us improved SEO ranking on our blog posts
    - and more….

In order to achieve this, we have to add a config in **nuxt.config.js.** Go to nuxt.config.js

```jsx
export default {
  target: 'static',
	// rest of your config
	....
}
```

Setting *target* to *static* allows us to statically generate routes using `nuxt generate`, hence hopping on Nuxt SSG benefits. 

Read More [https://nuxtjs.org/docs/features/deployment-targets](https://nuxtjs.org/docs/features/deployment-targets)

### Next, we create our custom routes

Nuxt provides us with a *generate* property in its config, that allows set how we would like to generate our static.

But we are more interested in the ***routes*** property of the ***generate*** config. ***Routes***, allows us to list additional routes we would like to add to the already existing routes in our ***pages*** directory

Read More about **Routes**: [https://nuxtjs.org/docs/configuration-glossary/configuration-generate#routes](https://nuxtjs.org/docs/configuration-glossary/configuration-generate#routes)

Read More **Generate**: [https://nuxtjs.org/docs/configuration-glossary/configuration-generate](https://nuxtjs.org/docs/configuration-glossary/configuration-generate)

Lets open nuxt.config.js

```jsx
import axios from 'axios'

export default {
// rest of your config,
generate: {
    routes() {
      const WP_ENDPOINT = 'YOUR_WORDPRESS_URL/wp-json/wp/v2/posts'
      const MAX_WP_PER_PAGE = 100
      const url = new URL(WP_ENDPOINT)
      url.searchParams.set('per_page', MAX_WP_PER_PAGE)

      return axios.get(url.toString()).then(({ data: allPosts }) => {
        const filterPosts = allPosts.filter((post) => post.status === 'publish')
        return filterPosts.map((post) => {
          return {
            route: `/blog/${post.slug}`,
            payload: { post },
          }
        })
      })
    },
  },
}
```

Explanation:

- We import ***axios*** so we can make easy API call, you can update it to FETCH if you’d like that.
- Next, we get all our posts from the WordPress site using a pagination of 100 posts per page.
- We then proceed to filter out posts that **DO NOT** have the status of ‘**publish**’. We only want to show our users published articles.
- We then went a little further to return an array of route objects. A route can either be a string or an object. We chose an object so that we can send a payload to the page. A payload in this instance is the data we need to be rendered on the page, which is a single post data. Also, the route is customized using the post slug such that we can have ***`blogs/seun-is-boy`***.This way is easier for users to get insight to the article by seeing the URL scheme

## Single Blog page

In the single blog page, you can have access to the payload which houses the data for the post by using **asyncData** (asyncData gives us access to data on the server-side, which is where this page will be generated at build time).

Payload is injected into the Nuxt Context for the single blog post page.

```jsx
async asyncData({ payload }) {
	const { post } = payload

	if(!post) {
		return {
			post: null
		}
	}

	// using post data to make API requests to get Author, Featured Media, and more

	const { data: author } = await axios.get(`YOUR_WORDPRESS_URL/wp-json/wp/v2/users/${post.author}`)
	const { data: media } = await axios.get(`YOUR_WORDPRESS_URL/wp-json/wp/v2/media/${post.featured_media}`)

	// get post category. It's important to check if *post.category.length > 0* before you make the api call
	const { data: category } = await axios.get(`YOUR_WORDPRESS_URL/wp-json/wp/v2/users/${post.categories[0]}`)

	return {
		post,
		author,
		media,
		category
	}
}
```

Now that we have all the post data needed to render info on the single blog post page. 

### Finally, what is left is just SEO

We can take advantage of the head Options API from Nuxt to generate our meta-information about the single post page

```jsx
head() {
	if(!this.post) {
		return {
			title: 'Post not found'
		}	
	}
	
	return {
		title:  this.post.title,
		meta: {
			{
        hid: 'description',
        name: 'description',
        property: 'description',
        content: this.post.yoast_head_json.og_description,
      },
      {
        hid: 'og:description',
        name: 'og:description',
        property: 'og:description',
        content: this.post.yoast_head_json.og_description,
      },
      {
        hid: 'og:title',
        name: 'og:title',
        property: 'og:title',
        content: this.post.yoast_head_json.og_title,
      },
      {
        hid: 'twitter:title',
        name: 'twitter:title',
        property: 'twitter:title',
        content: this.post.yoast_head_json.og_title,
      },
      {
        hid: 'twitter:description',
        name: 'twitter:description',
        property: 'twitter:description',
        content: this.post.yoast_head_json.og_description,
      },
      {
        hid: 'twitter:image',
        name: 'twitter:image',
        property: 'twitter:image',
        content: this.post.yoast_head_json.og_image[0].url,
      },
      {
        hid: 'og:url',
        name: 'og:url',
        property: 'og:url',
        content: `https://{YOUR_WEBSITE_DOMAIN}/blog/${this.post.slug}`,
      },
      {
        hid: 'twitter:url',
        name: 'twitter:url',
        property: 'twitter:url',
        content: `https://{YOUR_WEBSITE_DOMAIN}/blog/${this.post.slug}`,
      },
		}
	}
}
```

### Gotchas

- **hid** is the name assigned to the meta tag in the head property of nuxt.config.js. **`hid`** allows us to be able to change the values for these tags as needed.
    
    *Read more* [https://nuxtjs.org/docs/features/meta-tags-seo/](https://nuxtjs.org/docs/features/meta-tags-seo/)
    
- The ***twitter:image*** meta property  should abide by this rule. I recommend a 512 by 512 image dimension.
    
    > A URL to a unique image representing the content of the page. You should not use a generic image such as your website logo, author photo, or other image that spans multiple pages. Images for this Card support an aspect ratio of 1:1 with minimum dimensions of 144x144 or a maximum of 4096x4096 pixels. Images must be less than 5MB in size. The image will be cropped to a square on all platforms. JPG, PNG, WEBP and GIF formats are supported. Only the first frame of an animated GIF will be used. SVG is not supported.
    > 
    
    *Source*: [https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/summary](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/summary)
    

### Advise

- Use [https://cards-dev.twitter.com/validator](https://cards-dev.twitter.com/validator) to test what the blog posts would like when shared on social media WhatsApp, Twitter, Slack etc really.
- Build using **`nuxt generate`** then **`nuxt start`** on your local machine. Platforms like Vercel, and Cloudflare pages can handle production deployments for you with ease with fast built time.

### Future

- I intend to work on generating routes for more than 100 items. Right now routes can be generated for 100 posts on page 1 i.e {page: 1, perPage: 100}.
    
    **100** is the maximum amount of items per page that can be returned on WordPress API.
