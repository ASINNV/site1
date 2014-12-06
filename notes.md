# Dev Notes

## HTML

There are two main types of general structure that sites usually follow:

* All content is within one centered column, there is a background of some kind behind it
* There are multiple "sections" that take up the full width of the page, content is centered at a certain width within those sections.

This site follows the latter design style: You have full-width sections with fixed-width columns inside each one to keep content centered and readable. The simplest way to accomplish this in HTML is usually something like this:

```html
<section>
  <div class='container'>
    <!-- Content goes here -->
  </div>
</section>
```

Then in you will make sure the `.container` class does not get too wide and is centered.

```css
.container {
  width: 800px;
  margin: 0 auto;
}
```

---

When there is nothing inside a tag put it all on one line:

```html
<div class="gradient"></div>
```

---

Indentation is key to making HTML readable. Make sure you indent everything to where it should be.

This...

```html
<section class="midBox">
  <div class="textBox">
    <h1 class="tightle">What People Are Saying</h1>
    <div class="firstPair">
    <h1 class="firstComment">&ldquo;A Breakthrough Performance&rdquo;</h1>
    <h2 class="commenter"> - Everyone</h2>
    </div>
    <div class="secondPair">
    <h1 class="secondComment">&ldquo;Breathtaking&rdquo;</h1>
    <h2 class="commenter"> - Some Guy</h2>
    </div>
    <div class="thirdPair">
    <h1 class="thirdComment">&ldquo;Like Watching a live sex tape&rdquo;</h1>
    <h2 class="commenter"> - Roger Ebert</h2>
    </div>
  </div>
</section>
```

... Should be this:

```html
<section class="midBox">
  <div class="textBox">
    <h1 class="tightle">What People Are Saying</h1>
    <div class="firstPair">
      <h1 class="firstComment">&ldquo;A Breakthrough Performance&rdquo;</h1>
      <h2 class="commenter"> - Everyone</h2>
    </div>
    <div class="secondPair">
      <h1 class="secondComment">&ldquo;Breathtaking&rdquo;</h1>
      <h2 class="commenter"> - Some Guy</h2>
    </div>
    <div class="thirdPair">
      <h1 class="thirdComment">&ldquo;Like Watching a live sex tape&rdquo;</h1>
      <h2 class="commenter"> - Roger Ebert</h2>
    </div>
  </div>
</section>
```

### Headers

Using that same html from above as an example, be aware of where you use certain headers. There are different opinions on this, so my way of doing things isn't the way _everyone_ does it, so once you are more comfortable with code you can decide for yourself.

Try to only use one single `h1` per page. For the landing page, this is usually the main title (In our case "Adrian Sinnott"). That text is more important than anything else, and deserves to stand out in code as the only `h1`.

`h2`s are good for section titles, such as the "What people are saying" text you see in the HTML above. I would change that to an H2

```html
<h1 class="tightle">What People Are Saying</h1>
```

Then within the testimonials you would make the testimonial text an `h3`. The "commenter" text could be anything you like, but in this case I might just go with a `p` tag.

```html
<div class="firstPair">
  <h3 class="firstComment">&ldquo;A Breakthrough Performance&rdquo;</h3>
  <p class="commenter"> - Everyone</p>
</div>
```

### Classes & Specificity

Let's look at this code, which I already updated based on the recommendation above:

```html
<div class="firstPair">
  <h3 class="firstComment">&ldquo;A Breakthrough Performance&rdquo;</h3>
  <p class="commenter"> - Everyone</p>
</div>
<div class="secondPair">
  <h3 class="secondComment">&ldquo;Breathtaking&rdquo;</h3>
  <p class="commenter"> - Some Guy</p>
</div>
<div class="thirdPair">
  <h3 class="thirdComment">&ldquo;Like Watching a live sex tape&rdquo;</h3>
  <p class="commenter"> - Roger Ebert</p>
</div>
```

There are quite a few unecessary classes being used here. The reason is that you don't need to be so specific. Every one of these comments is styled exactly the same, so there is no reason you would ever need to style `.secondPair` or `.secondComment` in your CSS.

But since these are all exactly the same they do warrant a class assigned to them. It should be anything that makes sense to you, like "comment":

```html
<div class="comment">
  <h3>&ldquo;A Breakthrough Performance&rdquo;</h3>
  <p> - Everyone</p>
</div>
<div class="comment">
  <h3>&ldquo;Breathtaking&rdquo;</h3>
  <p> - Some Guy</p>
</div>
<div class="comment">
  <h3>&ldquo;Like Watching a live sex tape&rdquo;</h3>
  <p> - Roger Ebert</p>
</div>
```

Now you have much cleaner code that will be much easier to style. More importantly, if you decide you want to add a fourth comment by someone else it is really easy. You would just copy one of the comments above and change the content text. You would not need to create strange new classes like `.fourthPair` or `.fourthComment`.

---

For the footer the same general notes apply: Don't use non-generic container classes, they aren't usually necessary and if you need something specific to the footer you can simply style the `footer` element itself or add another class on top of the existing container class.

---

## CSS

### Global Styles

Whenever you style a base tag (i.e. `h1`, `p`, `a`, etc) you are setting that tags style across the _entire_ site. This is super powerful, but make sure you don't put super specific styles into global selectors.

You have a lot of this in your code, here's an example:

```css
img {
	position: absolute;
	top: 140px;
}
```

My guess is that if you had any other images on the site, you wouldn't want them to have an absoulte position and be be 140px from the top of the container.

The solution is to either style them within another class/id:

```css
.topBox img {
	position: absolute;
	top: 140px;
}
```

Or simply add a class/id to the image in question and then style that. When building larger sties that have multiple pages, you will find it super annoying if you have to go overwrite specific styles down the road. If you had to overwrite the positioning of every image on the page _other than_ the banner image it would be super annoying, tedious, and just plain ugly code.

### Height

As a general rule, it's very rare to actually set a specific height on an element. This is because by default elements will expand vertically to provide enough height for everything you put inside them. It's very flexible this way. 

That is not to say setting a fixed height isn't useful, but in the majority of cases it's not what you want. As you saw with the gradient you wanted to set it's height specifically to get the gradual gradient the design calls for.

One caveat is when you float elements. Be default this will cause the div that holds the elements to collapse, which is insane and probably a mistake on the part of the people who designed CSS. To fix this either apply `overflow: hidden;` to the div, or use a clearfix technique (google that if you need to).

### Hard pixel values

What I mean by this is things like `100px`: any hard value that determines width, height, etc. When you're just starting to build sites its fine to use hard values, but once you start building more advanced stuff or taking client projects you will need to be able to make responsive site. Building a site that responds well to a mobile screen means you will mostly be using percentages instead of pixels, because a fixed pixel width is not flexible. This is just a heads up.

---

## Overall Feedback

**DRY KISSes**

**D**on't **R**epeat **Y**ourself. This is the mantra of skilled coders and it applies to CSS and HTML as well as full-blown coding languages. For instance, don't create 5 different container classes when you can just make one and apply it to all elements. When you create a header for a site make sure to only style it once, unless it is different on different pages. Simple stuff like this is just a brief intro into what makes code beautiful (although HTML is never actually beautiful). 

**K**eep **I**t **S**imple **S**tupid. This is another coder mantra you will Hera sometimes, and it often goes hand-in-hand with DRY coding. Basically when add complexity to a codebase when you can get the same functionality with less, simpler, more readable code? When I say use generic classes, or simply omit classes when they aren't necessary, you are getting the same result with less code and you are keeping things simple. Simple (yet still effective) code is easy to read, and will still make sense if you view it a year from now after having completely forgotten all about the project.

**Creating a website from a mockup**

When you're copying from a mockup pay attention to _all_ the tiny details. In the current site the most obvious is the payment for. It does not look like the mockup. That's fine right now, but just something to note. If someone pays you to make a site for them based on a mockup they would be pretty dissappointed if they saw a form that looked different from their design. 

Also look at line height (the space between lines in a paragraph). You can set this property with CSS. Invariable the default line height in a browser is too low for easy reading, and you will have to specify a greater number. Take a look at the text next to the form in the design, and then look at the actual site. The line spacing is much less on the actual site than in the design.

Another note on this front: Design software does not always translate directly to CSS. Photoshop is notorious for this. If you take all the font-sizes, line-heights and other measurements direclty from a PSD file and code it up in CSS you will find that it looks different than the design. Just keep that in mind, because your job is not to simply copy values and make them work in CSS, it's to copy the appearance _exactly_. This means that if you need to deviate from the values provided in the mockup that's fine, just make it look the same in the browser as it does in the mockup.

