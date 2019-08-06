name: none
layout: true

---

name: normal
layout: true
class: left, middle

.footnote[[https://fitzgen.github.io/rust-conf-2019](https://fitzgen.github.io/rust-conf-2019)]

---

class: middle, center

# <big>Flatulence, Crystals, and Happy Little Accidents</big>
<br/>
### Nick Fitzgerald
### [@fitzgen](https://twitter.com/fitzgen)

???

* hi
* thanks for coming to my talk about algorithmic art and Rust
* back up first and give you some context to my story

---

class: middle, center

![Ferris the Crab with a WebAssembly Hard Hat](images/wasm-ferris.png)

???

* I do Rust things for work
    * these days on the intersection of Rust and WebAssembly
    * make fast things
    * make robust/reliable things
* create tools with good developer experiences, supporting many use cases
    * tools to help others build fast, robust, reliable things
* and I love my job!
    * but sometimes it can feel like a lot of Serious Business
* in my spare time, also writing little crates and side projects in Rust for fun
    * but these side projects had a tendency to become a part of my work
* realized that I was actually just working all the time
* not a recipe for a well-rounded life

---

class: center, middle

<img alt="Throwing on a pottery wheel" src="images/Black_and_white_pottery.jpg" class="centermiddle"/>

.headnote[Source: <a href="https://commons.wikimedia.org/wiki/File:Black_and_white_pottery_(Unsplash).jpg)">Wikimedia</a>]

???

* took up pottery as a hobby
* shaping the clay with my hands appealed to me
* I wanted a hobby that couldn't turn into work
    * something that doesn't have stakeholders
    * something that is just for me
    * something that I can relax and have fun doing

---

class: center, middle

<img alt="Stone with copper and a glazed mug with copper sand" src="images/bluepotter.jpg" class="centermiddle"/>

.headnote[Source: [instagram.com/bluepotter](https://www.instagram.com/p/BPJEw4nhqfU/)]

???

* what I really liked about pottery:
    * every facet of it has so much depth you could spend a lifetime exploring it
        * if you love throwing on the wheel, you can perfect throwing long,
          skinny, fragile vases
        * or you can make your own glazes from scratch, like what was done with
          this image

---

# TODO: image of my metallic green bowl

???

* with glaze in particular, the whole felt greater than the parts
    * there is an element of surprise
        * because glaze is often dull and chalky before you fire it
        * comes out glassy and colorful
    * you can combine glazes by mixing or layering
        * I discovered that if I put a coat of black on top of green, then
          * I got something that came out metallic and shimmery
          * almost like an oil slick
          * neither glaze alone had this property, but combine them and
            something new/surprising _emerges_
          * I tried to recreate this effect with five bowls
          * only a single bowl made it through the process
* pottery is very difficult and requires lots of patience
    * things can irreparably break at every step of the whole process
    * there is no undo or version control
    * this difficulty is part of the craft and what makes it so impressive!
* personal realization:
    * despite all these things about pottery that I really enjoyed,
    * I was getting more frustrated by pottery's difficulty than I was relaxing
      and having fun, which were my original goals

---

class: center, middle

<img alt="Process 13 (A) by Casey Reas, 2010" src="images/casey-reas-process-13.jpg" class="centermiddle"/>

.headnote[Source: [*Process 13 (A)* by Casey Reas, 2010](http://reas.com/p13_s/)]

???

* heard of "generative art" and "algorithmic art"
* where people would write programs that generate images, animations, or music
* the same way I enjoyed discovering how glazes could combine such that their
  sum was greater than the individual glazes,
* generative artists are often searching to program simple rules that combine to
  create emergent behavior that is more bigger than any single rule
    * for example, this is *Process 13 (A)* by Casey Reas
        * circles moving behind the scenes
          * but they're not drawn directly
        * whenever circles are touching, draw a line between their centers
    * there is no code that says "draw these trellises"
        * they emerge from the process to surprise and delight us
* however, while I appreciated this art when I saw it, I wasn't ever overcome by
  the urge to produce it myself
    * there was still something missing:
    * nothing to hold onto; nothing physical

---

<video autoplay loop class="centermiddle">
  <source src="images/axidraw-rust-5x-transposed.webm"/>
  <p>Video not supported.</p>
</video>

???

* and then I discovered pen plotters
* a pen plotter is a robot
    * you put a pen in its hand
    * and then you tell it to draw something
    * and this is the important part: and then it actually draws it!
    * now you have a physical, pen and paper artifact of your algorithm!!

---

.headnote[Source: [*Entropy Variation* by Paul Rickards, 2019](https://twitter.com/paulrickards/status/1133489029515751425)]

<img class="float-left width-50" alt="Entropy Variation by Paul Rickards, 2019" src="images/paul-rickards-entropy-variation.jpeg"/>
<img class="float-left width-50" alt="Close up of Entropy Variation by Paul Rickards, 2019" src="images/paul-rickards-entropy-variation-close-up-0.jpeg"/>
<img class="float-left width-50" alt="Another close up of Entropy Variation by Paul Rickards, 2019" src="images/paul-rickards-entropy-variation-close-up-1.jpeg"/>

???

* in addition to creating an artifact you can frame and give to someone, the
  physical leaves more room for _emergence_:
    * a pen doesn't draw perfect Euclidean lines with zero area
    * different pens draw different lines
        * color
        * width
        * ballpoint pens vs fountain pens vs markers vs gel pens
    * ink bleeds
        * move the pen slower -> more bleeding
    * colors combine
        * draw a blue line on top of a yellow one -> get a shade of green that
          you don't even have a marker for
    * I think this piece by Paul Rickards takes advantage of the medium superbly
        * the way that the rectangles overlap and the colors combine suggests a
          transparency that isn't really there
* _this_ is what I was missing from generative art
    * I'm hooked
* I made my decision:
    * I want to make algorithmic art
    * and I'm going to bring it into the physical world with a pen plotter

---

# How do I use a pen plotter?

---

<img class="centermiddle invert-80" src="images/bitmap-vs-svg.svg" />

.headnote[Source: [Wikipedia](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics#/media/File:Bitmap_VS_SVG.svg)]

???

* TODO

---

```
use svg::node::element;

let data = element::path::Data::new()
    .move_to((10, 10))
    .line_to((60, 20))
    .line_to((25, 60))
    .close();

let path = element::Path::new()
    .set("fill", "none")
    .set("stroke", "blue")
    .set("stroke-width", 3)
    .set("d", data);

let document = svg::Document::new()
    .set("viewBox", (0, 0, 70, 70))
    .add(path);

svg::save("triangle.svg", &document)?;
```

.headnote[[crates.io/crates/svg](https://crates.io/crates/svg)]

???

* the `svg` crate gives us a nice builder-style API for creating SVGs
* and we can get surprisingly far using just this crate
* this example draws a triangle
* the _path data_ describes a sequence of strokes in the coordinate space
* the _path element_ describes the styles of how the strokes should be drawn
* finally, the _document_ is the whole SVG image and its view box

---

<img class="centermiddle" alt="Screenshot of Saxi's user interface" src="images/saxi.png"/>

.headnote[[github.com/nornagon/saxi](https://github.com/nornagon/saxi)]

???

* to get the SVG to the Axidraw, use `saxi`
    * nice Web frontend to control the plotter
    * makes calibrating the pen's up and down height and centering on the page easy
    * can't recommend it highly enough

---

<pre><code class="svg">&lt;svg viewBox="0 0 70 70"
     xmlns="http://www.w3.org/2000/svg">
  &lt;path d="M10,10 L60,20 L25,60 z"
        fill="none"
        stroke="blue"
        stroke-width="3"/>
&lt;/svg>
</code></pre>

<div class="hbox">
  <img class="flex-1" alt="Software rendering of triangle.svg" src="images/triangle.svg"/>
  <span class="flex-1">TODO: plot triangle.svg</div>
</div>

???

* TODO

---

```
trait Rectangle {
    fn draw(
        &mut self,
        rng: &mut impl Rng,
        x: f64,
        y: f64,
        width: f64,
        height: f64,
        value: f64,
    ) -> element::path::Data;
}
```

???

* TODO

---

<img class="invert-80" alt="Rectangle 2" src="images/rectangle-2.png"/>

???

* TODO

---

<img class="invert-80" alt="Rectangle 6" src="images/rectangle-6.png"/>

???

* TODO

---

<img class="invert-80" alt="Rectangle 8" src="images/rectangle-8.png"/>

???

* TODO

---

```
trait Tiling {
    fn new(
        rng: &mut impl Rng,
        width: usize,
        height: usize,
    ) -> Self;

    fn get_value(
        &mut self,
        rng: &mut impl Rng,
        x: usize,
        y: usize,
    ) -> f64;
}
```

???

* TODO

---

<img class="invert-80 centermiddle" alt="Tiling 0 of Rectangle 10" src="images/tiling-0-rectangle-10.png"/>

???

* TODO

---

<img class="invert-80 centermiddle" alt="Tiling 3 of Rectangle 2" src="images/tiling-3-rectangle-2.png"/>

???

* TODO

---

class: middle, center

# <big><big>Thank You!</big></big>
<br/>
### Nick Fitzgerald
### [@fitzgen](https://twitter.com/fitzgen)
