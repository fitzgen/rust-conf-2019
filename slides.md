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
    * if you love throwing on the wheel, you can perfect throwing long, skinny,
      fragile vases
    * or you can make your own glazes from scratch, like what was done with this
      image

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
      * neither glaze alone had this property, but combine them and something
        new/surprising _emerges_
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

* TODO

---

.headnote[Source: [*Entropy Variation* by Paul Rickards, 2019](https://twitter.com/paulrickards/status/1133489029515751425)]

<!-- <div class="hbox centermiddle"> -->
  <img alt="TODO" src="images/paul-rickards-entropy-variation.jpeg"/>
  <!-- <div class="vbox"> -->
  <img alt="TODO" src="images/paul-rickards-entropy-variation-close-up-0.jpeg"/>
  <img alt="TODO" src="images/paul-rickards-entropy-variation-close-up-1.jpeg"/>
<!--   </div> -->
<!-- </div> -->

???

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
    .set("stroke", "black")
    .set("stroke-width", 3)
    .set("d", data);

let document = svg::Document::new()
    .set("viewBox", (0, 0, 70, 70))
    .add(path);

svg::save("triangle.svg", &document)?;
```

---

class: middle, center

# <big><big>Thank You!</big></big>
<br/>
### Nick Fitzgerald
### [@fitzgen](https://twitter.com/fitzgen)
