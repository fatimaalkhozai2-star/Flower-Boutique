# Design Forms in HTML, Media, and How the Browser Renders a Page



## Learning Objectives

By the end of this material you will be able to:

- Build accessible, well-structured HTML forms with the correct input types and validation attributes
- Explain the difference between `GET` and `POST` and choose between them
- Embed images, audio, video, and external content responsively and accessibly
- Describe what happens between typing a URL and seeing a page on screen
- Explain how the browser parses HTML into the DOM and CSS into the CSSOM
- Name the stages of the rendering pipeline and explain what triggers each one
- Clearly explain the roles of HTML, CSS, and JavaScript, and how they work together

---

# Part 1: Design Forms in HTML

## 1.1 Why Forms Matter

Forms are how users **send data** to a website: logging in, signing up, searching, uploading a file, paying, or writing a comment. Almost every real web application depends on forms.

A form does three jobs:

1. **Collects** data from the user (inputs)
2. **Validates** the data (rules such as "required" or "must be an email")
3. **Sends** the data to a server (or to JavaScript)

## 1.2 The `<form>` Element

```html
<form action="/register" method="post">
  <!-- form controls go here -->
</form>
```

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `action` | The URL that receives the data | `action="/login"` |
| `method` | The HTTP method used to send data | `method="post"` |
| `target` | Where to show the response | `target="_blank"` |


### GET vs POST

| | GET | POST |
|---|-----|------|
| Where is data sent? | In the **URL** (query string) | In the **request body** |
| Visible in the URL? | Yes | No |
| Bookmarkable / shareable? | Yes | No |
| Data size | Limited by URL length | Much larger |
| Typical use | Search, filters, reading data | Login, register, create, update, upload |
| Safe for passwords? | **Never** | Yes (together with HTTPS) |

Example of a GET request created by a search form:

```
https://example.com/search?q=html+forms&category=tutorials
```

> **Rule of thumb:** Use `GET` when the request only *reads* data. Use `POST` when it *changes* something or contains sensitive information.

## 1.3 The `<input>` Element and Its Types

`<input>` is the most important form control. The `type` attribute decides how it looks and behaves.

```html
<input type="text" name="username">
```

### Text-based types

```html
<input type="text"     name="fullname" placeholder="Your full name">
<input type="password" name="password">
<input type="email"    name="email"    placeholder="name@example.com">
<input type="tel"      name="phone">
<input type="url"      name="website">
<input type="search"   name="q">
```

### Number and range types

```html
<input type="number" name="age" min="16" max="99" step="1">
<input type="range"  name="volume" min="0" max="100" value="50">
```

### Date and time types

```html
<input type="date"           name="birthday">
<input type="time"           name="meeting_time">
<input type="datetime-local" name="appointment">
<input type="month"          name="start_month">
<input type="week"           name="week">
```

### Choice types

```html
<!-- Radio: choose ONE. Buttons with the same name belong to one group -->
<input type="radio" name="level" value="beginner"> Beginner
<input type="radio" name="level" value="advanced"> Advanced

<!-- Checkbox: choose ZERO or MORE -->
<input type="checkbox" name="hobby" value="coding"> Coding
<input type="checkbox" name="hobby" value="design"> Design
```

### Other types

```html
<input type="color"  name="favorite_color">
<input type="file"   name="avatar" accept="image/*">
```


> **Why use the right type?** On phones, `type="email"` shows an email keyboard, `type="tel"` shows a number pad, and `type="date"` shows a date picker. The browser also validates the value automatically.

## 1.4 The `name` Attribute (Very Important!)

The `name` attribute is the **key** in the data that gets sent. **An input without a `name` is not submitted.**

```html
<input type="text" name="username" value="balqees">
```

is sent as:

```
username=balqees
```

Common beginner mistake: using only `id` and forgetting `name`. The `id` is for CSS, JavaScript, and labels; the `name` is for sending data.

## 1.5 Labels and Accessibility

Every control needs a `<label>`. Labels help screen-reader users, and clicking the label focuses the input, which makes a bigger click area.

**Method 1: `for` and `id` (recommended)**

```html
<label for="email">Email</label>
<input type="email" id="email" name="email">
```

**Method 2: wrapping**

```html
<label>
  Email
  <input type="email" name="email">
</label>
```

> A `placeholder` is **not** a replacement for a `<label>`. Placeholders disappear when the user types and often have low contrast.

## 1.6 Other Form Controls

### `<textarea>`: multi-line text

```html
<label for="message">Message</label>
<textarea id="message" name="message" rows="5" cols="40" maxlength="500"></textarea>
```

### `<select>`: dropdown list

```html
<label for="country">Country</label>
<select id="country" name="country">
  <option value="">-- Select a country --</option>
  <optgroup label="Middle East">
    <option value="jo">Jordan</option>
    <option value="sa">Saudi Arabia</option>
  </optgroup>
  <optgroup label="Europe">
    <option value="de">Germany</option>
    <option value="fr">France</option>
  </optgroup>
</select>
```

Add `multiple` to allow selecting more than one option, and `selected` on an `<option>` to preselect it.

### `<datalist>`: suggestions while typing

```html
<label for="browser">Favorite browser</label>
<input list="browsers" id="browser" name="browser">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Safari">
  <option value="Edge">
</datalist>
```

The user can pick a suggestion **or** type their own value (unlike `<select>`).

### `<button>`

```html
<button type="submit">Create account</button>
<button type="reset">Reset</button>
<button type="button">Just a button (does not submit)</button>
```

`<button>` is more flexible than `<input type="submit">` because it can contain HTML (icons, spans). Inside a form, a `<button>` with no `type` behaves as **submit**, so always set `type` explicitly.


## 1.7 Built-in Validation

HTML validates data **before** it is sent, with no JavaScript needed.

| Attribute | What it does | Example |
|-----------|-------------|---------|
| `required` | Field cannot be empty | `<input required>` |
| `minlength` / `maxlength` | Minimum / maximum characters | `minlength="8"` |
| `min` / `max` | Minimum / maximum number or date | `min="18" max="60"` |
| `step` | Allowed increments | `step="5"` |
| `pattern` | Must match a regular expression | `pattern="[0-9]{10}"` |
| `type` | Email, URL, number, etc. are validated automatically | `type="email"` |

```html
<label for="phone">Phone (10 digits)</label>
<input
  type="tel"
  id="phone"
  name="phone"
  pattern="[0-9]{10}"
  title="Enter exactly 10 digits"
  required>
```

The `title` attribute is shown in the error message when the `pattern` fails.

### Styling valid and invalid fields with CSS

```css
input:valid   { border: 2px solid green; }
input:invalid { border: 2px solid red; }
input:focus   { outline: 3px solid dodgerblue; }
```

> **Important:** Browser validation improves the user experience, but it can be bypassed easily (for example with DevTools). **Always validate again on the server.**

## 1.8 Useful Attributes

| Attribute | Purpose |
|-----------|---------|
| `placeholder` | Hint text shown when the field is empty |
| `value` | Default or current value |
| `autofocus` | Focus this field when the page loads |
| `autocomplete` | Help the browser autofill (`name`, `email`, `username`, `new-password`, `current-password`) |
| `disabled` | Field is greyed out and **not submitted** |
| `readonly` | Field cannot be edited but **is submitted** |
| `multiple` | Allow several files, emails, or options |
| `accept` | Restrict file types (`accept=".pdf,.docx"`) |
| `checked` | Preselect a checkbox or radio |
| `form` | Connect a control to a form that is not its parent |

## 1.9 Uploading Files

A file upload requires **two things**: `method="post"` and `enctype="multipart/form-data"`.

```html
<form action="/upload" method="post" enctype="multipart/form-data">
  <label for="cv">Upload your CV (PDF)</label>
  <input type="file" id="cv" name="cv" accept=".pdf" required>
  <button type="submit">Upload</button>
</form>
```

## 1.10 What Happens When the User Clicks Submit?

```
User clicks "Submit"
        │
        ▼
Browser checks validation (required, pattern, type...)
        │
   ┌────┴─────┐
 Invalid    Valid
   │          │
   ▼          ▼
Show error   Build the data set from all controls that have a `name`
message      (skipping disabled fields and unchecked checkboxes/radios)
             │
             ▼
        Send the request (GET or POST) to `action`
             │
             ▼
        Server processes it and sends a response
             │
             ▼
        Browser shows the new page (or JavaScript handles it)
```


---

# Part 2: Media

## 2.1 Images: `<img>`

```html
<img src="images/team.jpg" alt="Students working together in the classroom" width="800" height="533">
```

| Attribute | Purpose |
|-----------|---------|
| `src` | Path or URL of the image (**required**) |
| `alt` | Text alternative (**required for accessibility**) |
| `width`, `height` | Intrinsic size in pixels; lets the browser reserve space and prevents layout shift |
| `loading="lazy"` | Load the image only when it is near the viewport |
| `decoding="async"` | Allow the browser to decode the image off the main flow |
| `srcset`, `sizes` | Provide multiple sizes for responsive images |

### File paths

```html
<img src="logo.png">                       <!-- same folder -->
<img src="images/logo.png">                <!-- subfolder -->
<img src="../images/logo.png">             <!-- one folder up -->
<img src="/assets/logo.png">               <!-- from the site root -->
<img src="https://example.com/logo.png">   <!-- external URL -->
```

### Writing good `alt` text

| Situation | Good `alt` |
|-----------|-----------|
| Informative image | `alt="Bar chart showing sales growth of 40% in 2025"` |
| Image that is a link | `alt="Go to home page"` (describe the destination, not the picture) |
| Purely decorative | `alt=""` (empty, so screen readers skip it) |
| Logo | `alt="DOT Jordan logo"` |

Avoid "image of..." or "picture of...": screen readers already announce it is an image.

### Choosing an image format

| Format | Best for | Notes |
|--------|----------|-------|
| **JPEG** | Photographs | Small files, no transparency |
| **PNG** | Screenshots, graphics with transparency | Lossless, larger files |
| **GIF** | Simple animations | Limited colors, heavy; prefer video |
| **SVG** | Logos, icons, illustrations | Vector: sharp at any size, tiny files, styleable with CSS |
| **WebP** | Photos and graphics on the web | Smaller than JPEG/PNG, wide support |
| **AVIF** | Photos when you want the smallest size | Excellent compression, needs a fallback for older browsers |


## 2.2 Responsive Images


The browser chooses the best file for the user's screen size and pixel density.

```html
<img
  src="images/hero-800.jpg"
   alt="Students at a coding bootcamp"
  width="800" height="533">
```

- `400w`, `800w`, `1600w` tell the browser the **actual width** of each file
- `sizes` tells the browser how wide the image will be **on the page**


### Making images fit in CSS

```css
img {
  max-width: 100%;   /* never overflow the container */
  height: auto;      /* keep the proportions */
}

.card img {
  width: 100%;
  aspect-ratio: 16 / 9;
  object-fit: cover; /* crop to fill without stretching */
}
```

## 2.3 Audio: `<audio>`

```html
<audio controls preload="metadata">
  <source src="audio/lecture.mp3" type="audio/mpeg">
  <source src="audio/lecture.ogg" type="audio/ogg">
  Your browser does not support the audio element.
</audio>
```

| Attribute | Purpose |
|-----------|---------|
| `controls` | Show play, pause, and volume controls |
| `autoplay` | Start automatically (blocked by most browsers unless muted) |
| `loop` | Repeat when finished |
| `muted` | Start silent |
| `preload` | `none`, `metadata`, or `auto`: how much to load in advance |

Common audio formats: **MP3** (widest support), **AAC/M4A**, **OGG**, **WAV** (uncompressed, large).

## 2.4 Video: `<video>`

```html
<video controls width="640" height="360" poster="images/video-cover.jpg" preload="metadata">
  <source src="video/intro.webm" type="video/webm">
  <source src="video/intro.mp4"  type="video/mp4">
  <track kind="subtitles" src="captions/intro-en.vtt" srclang="en" label="English" default>
  Your browser does not support the video element.
</video>
```

| Attribute | Purpose |
|-----------|---------|
| `controls` | Show the player controls |
| `poster` | Image shown before the video plays |
| `autoplay` | Play automatically (usually requires `muted`) |
| `muted` | Start with the sound off |
| `loop` | Repeat the video |
| `playsinline` | Play inside the page on iPhones instead of forcing full screen |
| `preload` | `none`, `metadata`, or `auto` |
| `width`, `height` | Reserve space in the layout |

Common video formats: **MP4 (H.264)**: best compatibility; **WebM (VP9/AV1)**: smaller files.

### A background video pattern

```html
<video autoplay muted loop playsinline>
  <source src="video/background.mp4" type="video/mp4">
</video>
```

> Browsers block autoplay with sound to protect users. If you want autoplay, the video must be **muted**.







---

# Part 3: The Browser Rendering Process

## 3.0 The Big Picture: From URL to Pixels

When you type `https://example.com` and press Enter, this happens:


---

## 3.1 Browser Parsing Process

**Parsing** means reading text and converting it into a structure the computer can work with.


```html
<h1 class="title">Hello</h1>
```

is turned into tokens like:

```
StartTag: h1   (attribute: class="title")
Character: H, e, l, l, o
EndTag:   h1
```

### The parser works incrementally

The browser does **not** wait for the whole file. It starts building the DOM as soon as the first bytes arrive (streaming). That is why a page can begin to appear while it is still downloading.

While parsing, a lightweight **preload scanner** looks ahead in the HTML and starts downloading resources it finds (images, stylesheets, scripts) early, before the main parser reaches them.

### The HTML parser is forgiving

Unlike many programming languages, HTML does not "crash" on mistakes. The browser follows well-defined rules to repair errors:

```html
<p>First paragraph
<p>Second paragraph
```

The browser closes the first `<p>` automatically, and the result is two separate paragraphs. Missing `<html>`, `<head>`, or `<body>` tags are added automatically too. Still, **write valid HTML**: relying on error correction can cause unexpected results.

### Parsing CSS: building the CSSOM

CSS goes through the same kind of process:

```
CSS bytes → characters → tokens → nodes → CSSOM (CSS Object Model)
```

The **CSSOM** is a tree that stores every style rule and how it cascades and inherits.

```css
body   { font-size: 16px; }
h1     { color: navy; }
.title { font-weight: bold; }
```

```
CSSOM
└── body (font-size: 16px)
    └── h1 (font-size: 16px inherited, color: navy)
        └── .title (font-weight: bold)
```

### Parsing JavaScript

When the parser meets a `<script>`, the JavaScript engine (for example V8 in Chrome) takes over: it parses the code, compiles it, and executes it. JavaScript can **read and modify** both the DOM and the CSSOM, so it affects how the parser behaves.

### How resources affect parsing

| Resource | Blocks HTML parsing? | Blocks rendering? |
|----------|---------------------|-------------------|
| `<link rel="stylesheet">` | No, but it delays any script that follows | **Yes**: the browser will not paint until the CSS is ready |
| `<script src="app.js">` (no attributes) | **Yes**: parsing stops until the script is downloaded and executed | Yes, for the content after it |
| `<script async src="...">` | Only while it *executes* | Only while it executes |
| `<script defer src="...">` | **No**: it runs after parsing finishes | No |
| `<script type="module">` | No: behaves like `defer` by default | No |
| `<img>` | No | No (but a missing size can cause layout shifts) |

```html
<head>
  <link rel="stylesheet" href="styles.css">   <!-- render-blocking, keep it small -->
  <script src="app.js" defer></script>        <!-- recommended for most scripts -->
</head>
```


---

## 3.2 DOM Tree Creation

### What is the DOM?

The **DOM (Document Object Model)** is a **tree-like, in-memory representation** of your HTML page. It is what JavaScript actually works with. The DOM is **not** the same as your HTML source file: it is a live object that the browser builds and that scripts can change.

### From HTML to DOM: a worked example

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>My Page</title>
  </head>
  <body>
    <h1>Hello</h1>
    <p>Welcome to <strong>my</strong> page.</p>
    <img src="me.jpg" alt="Me">
  </body>
</html>
```

The DOM tree:

```
Document
└── html  (lang="en")
    ├── head
    │   └── title
    │       └── "My Page"          (text node)
    └── body
        ├── h1
        │   └── "Hello"            (text node)
        ├── p
        │   ├── "Welcome to "      (text node)
        │   ├── strong
        │   │   └── "my"           (text node)
        │   └── " page."           (text node)
        └── img  (src, alt)
```

### Node types

| Node type | Example | `nodeType` |
|-----------|---------|-----------|
| Document | the whole page | 9 |
| Element | `<p>`, `<div>` | 1 |
| Text | the words inside `<p>` | 3 |
| Comment | `<!-- note -->` | 8 |

Attributes (`class`, `id`, `src`...) are stored as properties of their element nodes.

### Tree vocabulary

- **Root**: the top node (`html` under `document`)
- **Parent / child**: `body` is the parent of `h1`; `h1` is a child of `body`
- **Siblings**: `h1`, `p`, and `img` share the same parent
- **Ancestor / descendant**: `html` is an ancestor of `strong`; `strong` is a descendant of `html`

### How the tree is constructed

The parser uses a **stack of open elements**:

1. See `<html>` → create the node, push it on the stack
2. See `<body>` → it becomes a child of `html`, push it
3. See `<p>` → it becomes a child of `body`, push it
4. See text → create a text node as a child of the top of the stack
5. See `</p>` → pop `p` off the stack
6. Continue until the end of the document

This is exactly why **proper nesting** matters: an unclosed or misplaced tag changes the parent of everything after it.

### The DOM is not the CSSOM (and not the Render Tree)

| Structure | Built from | Contains |
|-----------|-----------|----------|
| **DOM** | HTML | All elements, text, and comments |
| **CSSOM** | CSS | All style rules |
| **Render Tree** | DOM + CSSOM | Only the things that will be **visible**, with their computed styles |

---

## 3.3 Rendering Pipeline

Once the DOM and CSSOM exist, the browser turns them into pixels. This series of steps is the **rendering pipeline** (also called the **Critical Rendering Path**).

```
 HTML ──► DOM ─────┐
                   ├──►  Render Tree ──►  Layout  ──►  Paint  ──►  Composite ──► Screen
 CSS  ──► CSSOM ───┘     (Style)         (Reflow)      (Raster)     (GPU layers)
                            ▲
                            │  JavaScript can change the DOM/CSSOM at any time
                         JavaScript
```

### Step 1: DOM and CSSOM (covered above)

### Step 2: Style and the Render Tree


### Step 3: Layout (Reflow)

### Step 4: Paint


### Step 5: Composite


---

# Part 4: Difference Between HTML, CSS, and JavaScript

## The Three Layers of the Web

| | HTML | CSS | JavaScript |
|---|------|-----|-----------|
| **Stands for** | HyperText Markup Language | Cascading Style Sheets | (a programming language; not related to Java) |
| **Role** | **Structure and content** | **Presentation and layout** | **Behavior and logic** |
| **Answers** | "What is on the page?" | "How does it look?" | "What does it do?" |
| **Type** | Markup language | Style sheet language | Programming language |
| **Has logic** (variables, loops, conditions)? | No | Very limited ( variables, media queries) | Yes |
| **Result if missing** | No page at all | Page works but looks plain | Page is static and not interactive |
| **Browser builds** | DOM | CSSOM | Modifies the DOM/CSSOM, talks to APIs |
| **File extension** | `.html` | `.css` | `.js` |


---
## Task
Build a Café Website ☕️
Create a simple Café Website using HTML only.
Your website should include the following:
Requirements
1. Page Structure
 - Create a proper HTML document structure.
- Add a suitable `<title>`.
- Use different heading tags such as `<h1>`, `<h2>`, and `<h3>`.
- Add paragraphs using `<p>`.
2. Café Information
- Add the café name.
- Add a short description about the café.
- Add an image of the café or one of its products using `<img>`.
3. Menu


 - Create a simple café menu.
- Add at least 5 food or drink items.
- Use lists such as `<ul> or <ol>`.
4. Links


 - Add links using `<a>`.
- Include at least one external link.
- Add navigation links to different sections of your page using id.
5. Media


- Add at least 2 images.
- Add an audio using `<audio>`.
- Add a video using `<video>`.
- Make sure the audio and video have controls.
6. Customer Form
Create a simple Customer Feedback Form that includes:


- Name
- Email
- Rating
- Favorite drink
- Comments
-Submit button
7. Use appropriate form elements such as:
`<form><label><input><select><option><textarea><button>`
2. HTML Attributes
Use appropriate attributes throughout your page, such as:

`
id
href
src
alt
type
placeholder
required
controls`
- Focus on using the correct HTML tags and attributes.