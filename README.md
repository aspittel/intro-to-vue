# Vue Vixens DC's Hello World Meetup - Intro to Vue

![Vue Vixens DC Logo](https://pbs.twimg.com/profile_images/1070775214370373633/borvu2Xx_400x400.jpg)

Vue.js is a frontend framework that is optimized for **progressive integration**. That means you can have a large app with only a couple Vue components integrated -- or you could start from scratch and work completely within the Vue ecosystem.

Another thing that sets Vue apart is the lower learning curve than a lot of frameworks. Instead of having to understand complex topics, if you know HTML, CSS, and JavaScript, you're already pretty close!

Like any framework, it adds a structure and utilities to your frontend so that your app is easier to extend as it grows, is more organized, and you don't have to "reinvent the wheel" as often.

Vue is also really cool because it's ecosystem is really well integrated -- a lot of the utilities that would normally be 3rd party libraries are built by the Vue core maintainers, like [Vue Router](https://router.vuejs.org/) and [Vuex](https://vuex.vuejs.org/).

Throughout this workshop, we'll explore the key features of Vue, and create an app together!

Here's what we'll be building, though with some more interactive features. The like button will toggle from the heart outline to the red heart based on user clicks. Also, the character number will count down when someone types in the text box.

finished product - https://codepen.io/aspittel/pen/oVMBmO

starting app - https://codepen.io/aspittel/pen/oVMYqG

Go ahead and check out the HTML and CSS code above, we'll be building off of the HTML with our Vue code.

## Setting up a Vue App

For now, we'll use a Vue CDN -- we want a minimalist setup. In the future, you may want a more extensive environment, in which case you can use the [Vue CLI](https://cli.vuejs.org/).

Go to the `settings` button on Codepen, switch to the JavaScript tab, and search for Vue on CDNjs. This adds the Vue library to our project, so we can use all of the methods and features that Vue gives us.

Now, we need to create a Vue instance and attach it to our HTML in order to fully integrate Vue!

Let's create a `const` that stores our `Vue` instance.

```js
const app = new Vue()
```

We're going to pass an object when we create this Vue app, it'll have all our configuration and application logic for now.

The first thing we're going to add to that object is `el` -- which is the element that we want to be the base of our Vue app. In this case the element with the `status` class.

```js
const app = new Vue({
  el: ".status"
})
```

Then, we'll add our `data`. To test this out, let's add the `tweetText` as data -- so where we have `Hello World!` right now will become a variable. Down the road we're going to make more tweets with different text, so it makes sense to make that piece of the tweet dynamic.

```js
const app = new Vue({
    el: ".status",
    data: {
        tweetText: "Hello World!"
    }
})
```

When we want to add more dynamic data (or data that will change within our Vue app) we'll add more attributes to this `data` object.

Now, we can use our newly created data in our HTML and plug in the variables that way! If you've ever used Handlebars or another templating language, it's kind of like that.

If you go to the hardcoded "Hello World!" in the HTML, we can now replace it with `{{tweetText}}` which will pull from our Vue data!

```html
<p class="tweet-text">
  {{ tweetText }}
</p>
```

Try to change your `tweetText` in Vue, and it'll change in your output as well!

Let's brainstorm for a second on what other data we have that will change within the course of our app.

- The heart will toggle between liked and unliked
- Our characters remaining will decrease when we type in the

<!--
Let's go ahead and add attributes for those in our `data` object.

```diff
data: {
    tweetText: :"Hello World!",
+    charactersRemaining: 280,
+    liked: false
}
```

We'll also make `charactersRemaining` dynamic in the HTML.

```html
<span class="characters-remaining">
  {{ charactersRemaining }} characters remaining
</span>
```
We'll hold off on the `liked` attribute for now, we'll come back to that in a second.
-->

### Your turn: Add attributes for the `charactersRemaining` and `liked`. Add the `charactersRemaining` to the HTML too!

## Methods

Now that we have our data, we need to make it update based on user actions.

We're going to add another attribute to our Vue object -- this one will store our methods.

```js
const app = new Vue({
    el: ".status",
    data: {
        tweetText: "Hello World!",
        charactersRemaining: 280,
        liked: false
    },
    methods: {}
})
```

We have two "actions" for our app -- toggling the like and changing the characters remaining number when the user types. Let's work on the character counting first.

We'll add a method to our methods object first:

```js
methods: {
    countCharacters: function() {

    }
}
```

Let's think about the logic for this function: we need to count how many characters the user has typed into the `textarea`. Then, we need to subtract that count from 280 (or our character limit).

Let's create a data attribute for the comment text, and then update that every time the user types in the `textarea`.

```diff
  data: {
    tweetText: 'Hello World!',
    charactersRemaining: 280,
+    commentText: '',
    liked: false
  },
```

```html
<textarea placeholder="tweet your reply" v-model="commentText"></textarea>
```

`v-model` is a _directive_ that syncs our data attribute with what the user has typed into the `textarea`. So no matter how much or little they have typed in, `commentText` will match what they've typed. To take one quick step back, _directives_ are HTML attributes that are provided by Vue, they're prefixed by `v-`.

Okay, now back to our method. We can access our data in our methods with `this.myDataAttribute` ([here's](https://codeburst.io/all-about-this-and-new-keywords-in-javascript-38039f71780c) a great reference on JavaScript's `this`).

So, we can update the `charactersRemaining` with the following logic:

```js
methods: {
    countCharacters: function() {
        this.charactersRemaining = 280 - this.commentText.length
    }
}
```

Now, we need to make sure that `countCharacters` runs every time the user types in the `textarea`.

Luckily, Vue has the `v-on` directive, and we can add the event after it so that we run the method each time that event takes place. In this case, `v-on:input="countCharacters` will run the `countCharacters` method each time the user types in the `textarea`.

```html
<textarea
  placeholder="tweet your reply"
  v-model="commentText"
  v-on:input="countCharacters"
></textarea>
```

Okay, now let's step back and work on our `toggleLike` method.

### Your turn: Create a `toggleLike` method!

> Hint: instead of running this event on input, we want to run it on click!

Don't worry about changing emojis quite yet, we'll go over conditionals together in a minute!

<!--
We first need to add the method to our `methods` object.

```js
methods: {
    ...
    toggleLike: function () {

    }
}
```

The body of the method should change `this.liked` to the opposite of what it currently is. So:

```js
toggleLike: function () {
    this.liked = !this.liked
}
```

Okay, now we need to make that action run.

On our `reactions` div, let's add an event listener.

```html
<div class="reactions like" v-on:click="toggleLike">
  ...
</div>
```
Now, it's time to introduce another Vue feature: conditionals!
-->


## Conditionals

Vue allows us to conditionally render data with the `v-if` directive.

Let's add the following span-wrapped emoji within our `reactions` div:

```html
<span v-if="liked">♥️</span>
```

Now, our red heart emoji only shows up if `liked` is `true`. Let's also add a `v-else` to our heart outline emoji, so that it only renders if `liked` is `false`.

```html
<span v-if="liked">♥️</span> <span v-else>♡</span>
```

Yay! Now our likes work!

If you had any issues with the above steps, here's a Codepen with what we have so far.

https://codepen.io/aspittel/pen/WmKRNbG

Now that we have our interaction down, how would we create a bunch more tweets with the same functionality but different state and text? Components!

## Components

Similar to other frontend frameworks, Vue apps are broken down into components. We compose components together in order to create full user interfaces. A good rule of thumb is that if a chunk of the user interface is used multiple times, it should be broken into a component.

In a production application, our tweet would probably be broken into subcomponents -- we may have a component for the comment text area, one for the like functionality, one for the profile picture, etc. But, for tonight, we will just make the full tweet into a component so that we can easily create a bunch more tweets.

First, let's move the logic from our Vue instance into a component.

The first argument to `Vue.component` is the name of the component, in this case "tweet". We're also turning data into a function that returns an object. This allows us to have multiple `tweet` component instance, each with separate data.

```js
Vue.component("tweet", {
  data: function() {
    return {
      charactersRemaining: 280,
      commentText: "",
      liked: false
    }
  },
  methods: {
    countCharacters: function() {
      this.charactersRemaining = 280 - this.commentText.length
    },
    toggleLike: function() {
      this.liked = !this.liked
    }
  }
})
```

We also need the `template` for the component -- or the HTML that the component will render. We're going to grab all of the existing HTML and paste into a template attribute on our component.

```js
template: `<div class="status">
  <div class="tweet-content">
    <img src="https://pbs.twimg.com/profile_images/1070775214370373633/borvu2Xx_400x400.jpg" class="logo" alt="Vue Vixens DC logo">
    <div class="tweet">
      <a href="https://twitter.com/vuevixensdc">Vue Vixens DC</a>
      <span>@VueVixensDC · Mar 20</span>
      <p class="tweet-text">
        {{ tweetText }}
      </p>
      <div class="reactions">
        <span v-on:click="toggleLike" class="like">
          <span v-if="liked">♥️</span>
          <span v-else>♡</span>
        </span>
      </div>
    </div>
  </div>
  <div class="comment-bar">
    <textarea placeholder="tweet your reply" v-model="commentText" v-on:input="countCharacters">
    </textarea>
    <span class="characters-remaining">
      {{ charactersRemaining }} characters remaining
    </span>
  </div>
</div>`
```

Now, we have a Vue component!

One other quick thing we need to add: the tweet text is going to be different from tweet to tweet. We'll pass in different tweet text for each individual tweet through `props` -- which allow us to pass data to a component from outside of that component. For now, we'll just specify that our component has a prop associated with it.

```js
Vue.component('tweet', {
  props: ['tweetText'],
...
})
```

We still have to have a Vue app though, so let's add that back into our JavaScript:

```js
new Vue({ el: "#app" })
```

Cool, now our JavaScript is set, we just have to handle our HTML. In our Vue instance, we're looking for an element with the id `app` now, so let's create that.

```html
<div id="app"></div>
```

And, inside of our new Vue app, we'll add some instances of our tweet component.

```html
<div id="app">
  <tweet tweet-text="hello world!"></tweet>
  <tweet tweet-text="hi!"></tweet>
</div>
```

Notice how we're passing in our `tweetText` prop -- Vue converts the JavaScript camel case to kebab case in HTML. Outside of that change, our props look like HTML attributes.

Now our component should be good to go!

https://codepen.io/aspittel/pen/YgjNjb

One more quick thing though, usually instead of hardcoding each tweet in the HTML, we're going to want to loop through a data structure and create a tweet component for each of those items. Let's look at how to do that in Vue!

We're going to go into our Vue app instance and add some tweet data.

```js
new Vue({
  el: "#app",
  data: {
    tweets: [
        { id: 1, tweetText: "hello world!" }, 
        { id: 2, tweetText: "hi!" }
    ]
  }
})
```

Now we'll use another Vue directive, `v-for` in order to loop through the tweets array and create a `tweet` instance for each!

```html
<div id="app">
  <tweet
    v-for="tweet in tweets"
    v-bind:key="tweet.id"
    v-bind:tweet-text="tweet.tweetText"
  ></tweet>
</div>
```

Notice that we use `v-bind` twice here -- it allows us to dynamically update html attributes (or use variables within them). Keys are recommended whenever you use `v-for` -- it allows Vue to identify the child elements better ([more](https://vuejs.org/v2/guide/list.html#key)).

Awesome! Now we can create more tweets by adding an element to the `tweets` array!

Here's all of that code together.

https://codepen.io/aspittel/pen/oVMBmO

## Next Steps

First, there's a lot of cool features that you can add to the widget we just built. You can make the profile pictures different from tweet to tweet, along with the date and user data. You can also disable or highlight overflow text in our textarea. You could even use the Twitter API to use real tweets and even make the comment posting work!

Here are some more awesome resources for continuing to learn Vue:

* [Vue Vixens on DEV](https://dev.to/vuevixens)
* [Sarah Drasner's Vue series](https://css-tricks.com/intro-to-vue-1-rendering-directives-events/)
* [The Vue Documentation](https://vuejs.org/)

## Keep in touch!


