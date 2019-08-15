name: none
layout: true

---

name: normal
layout: true
class: left, middle

.footnote[[fitzgen.github.io/rust-conf-2019](https://fitzgen.github.io/rust-conf-2019)]

---

# .purple[<big>Flatulence, <br/> Crystals, <br/> and Happy Little Accidents</big>]

### .green[Nick Fitzgerald] <br/> [@fitzgen](https://twitter.com/fitzgen)

???

* hi, I'm Nick Fitzgerald
* thanks for coming to my talk about algorithmic art and Rust
* before art+Rust, share journey of how got here

---

<img class="centermiddle" alt="Ferris the Crab with a WebAssembly Hard Hat" src="images/wasm-ferris.png"/>

???

* I do Rust things for work
    * intersection of Rust and WebAssembly
    * make fast things
    * make robust/reliable things
* create tools
    * w/ good developer experiences
    * supporting many use cases
    * help others build fast/robust/reliable things
* I love my job!
    * Serious Business
* in my spare time?
    * side projects in Rust for fun
    * but: semi related to work --> become part of work
* realized: just working all the time
* not a recipe for a well-rounded life

---

<img class="centermiddle" alt="Throwing on a pottery wheel" src="images/Black_and_white_pottery.jpg" class="centermiddle"/>

.headnote[Source: <a href="https://commons.wikimedia.org/wiki/File:Black_and_white_pottery_(Unsplash).jpg)">Wikimedia</a>]

???

* took up pottery as a hobby
* shaping clay w/ my hands was appealing
* I wanted a hobby that couldn't turn into work
    * no stakeholders
    * just for me
    * relax and have fun

---

<img alt="Stone with copper and a glazed mug with copper sand" src="images/bluepotter.jpg" class="centermiddle"/>

.headnote[Source: [Matt Fiske Ceramics](https://www.instagram.com/p/BPJEw4nhqfU/)]

???

* every facet has so much depth
    * love throwing? --> long/skinny/fragile vases
    * or make glaze from scratch, like this image

---

# TODO: image of my metallic green bowl

???

* glaze: whole > parts
    * element of surprise
        * before firing: dull/chalky
        * after: glassy and colorful
    * combine glazes: mixing / layering
        * discovered: black on top of green, then
          * metallic and shimmery; oil slick
          * neither glaze alone had property
          * combine --> something new/surprising _emerges_
          * tried: recreate w/ five bowls
          * only one survived
* b/c pottery is difficult / requires patience
    * can break at every step
    * no undo / version control
    * part of craft: impressive!
* personal realization:
    * despite things about pottery that I enjoyed,
    * more frustrated by difficulty than relaxing / having fun

---

<img alt="Process 13 (A) by Casey Reas, 2010" src="images/casey-reas-process-13.jpg" class="centermiddle"/>

.headnote[Source: [*Process 13 (A)* by Casey Reas, 2010](http://reas.com/p13_s/)]

???

* "generative/algorithmic art" = write programs to generate images/animations/music
* recognized similarity w/ surprise of combining glazes:
    * generative artists programming simple rules --> emergent behavior, bigger
      than any single rule
        * *Process 13 (A)* by Casey Reas
            * circles moving behind scenes
              * not drawn directly
            * circles touching --> draw a line
        * no code says "draw trellises"
            * emerge from process to surprise / delight us
* however:
    * could appreciate; not overcome w/ urge to make
    * nothing physical

---

<video autoplay loop class="centermiddle" alt="The AxiDraw pen plotter drawing the Rust logo">
  <source src="images/axidraw-rust-5x-transposed.webm"/>
  <p>Video not supported.</p>
</video>

???

* discovery: pen plotters
* pen plotter = robot
    * you put a pen in its hand
    * and then you tell it to draw something
    * and then -- important part -- draws it!
* now you have: physical, pen and paper artifact of generative algorithm!!

---

.headnote[Source: [*Entropy Variation* by Paul Rickards, 2019](https://twitter.com/paulrickards/status/1133489029515751425)]

<div class="hbox">
  <img class="flex-1 small-padding" alt="Entropy Variation by Paul Rickards, 2019" src="images/paul-rickards-entropy-variation.jpeg"/>
  <div class="vbox flex-1">
    <img class="flex-1 small-padding" alt="Close up of Entropy Variation by Paul Rickards, 2019" src="images/paul-rickards-entropy-variation-close-up-0.jpeg"/>
    <img class="flex-1 small-padding" alt="Another close up of Entropy Variation by Paul Rickards, 2019" src="images/paul-rickards-entropy-variation-close-up-1.jpeg"/>
  </div>
</div>

???

* physical medium = more variables / emergence:
    * pen can't draw Euclidean lines w/ zero area
    * different pens
        * width
        * color
        * ballpoint vs fountain vs markers vs gel
    * ink bleeds
        * move the pen slower --> more bleeding
    * colors combine
        * blue on yellow --> unique green
    * this piece by Paul Rickards
        * takes advantage of medium superbly
        * rectangles overlap / colors combine --> transparency / depth
* _this_ = what was missing
* decision:
    * make algorithmic art
    * bring into physical world w/ pen plotter
* bought AxiDraw

---

class: middle, center

# .yellow[How do I use a pen plotter?]

???

* more excitement than knowledge
* how to draw something?

---

<img class="center invert-90" style="max-height: 30vh" src="images/bitmap-vs-svg.svg" />

.headnote[Source: [Wikipedia](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics#/media/File:Bitmap_VS_SVG.svg)]

```svg
<svg viewBox="0 0 70 70"
     xmlns="http://www.w3.org/2000/svg">
  <path d="M10,10 L60,20 L25,60 z"
        fill="pink"
        stroke="blue"
        stroke-width="3"/>
</svg>
```


???

* easiest way = SVG
    * <u>S</u>calable <u>V</u>ector <u>G</u>raphics
* no pixels; shapes and paths
* shapes/paths described w/in abstract coordinate space
* pen plotter _scales_ coordinate space up / down --> size of paper
    * draws scaled shapes/paths/lines
* ask: know HTML / XML?
    * raise hands -- awesome!
    * like HTML / XML: SVG is text with pointy brackets
    * everyone who raised hand = on the way to being a plotter artist :)

---

```
use svg::node::element;

let data = element::path::Data::new()
    .move_to((10, 10))
    .line_to((60, 20))
    .line_to((25, 60))
    .close();

let path = element::Path::new()
    .set("fill", "pink")
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

* don't want: plot existing SVG
    * want: generate my own SVGs w/ Rust!
    * this code generated SVG on prev slide
* `svg` crate:
    * builder-style API
    * can get surprisingly far w/ just this crate
* this example = triangle
    * _data_ = sequence of strokes in coordinate space
        * ultimately controls pen movement
    * _path element_ = styles for strokes
    * _document_ = whole SVG image and box in coordinate space to render

---

<img class="centermiddle" alt="Screenshot of Saxi's user interface" src="images/saxi.png"/>

.headnote[[github.com/nornagon/saxi](https://github.com/nornagon/saxi)]

???

* send SVG --> AxiDraw? use `saxi`
    * nice Web frontend
    * calibrating pen's up / down height; centering
    * can't recommend it highly enough

---

<div class="hbox">
  <img class="flex-1" alt="Software rendering of triangle.svg" src="images/triangle.svg"/>
  <span class="flex-1">TODO: plot triangle.svg</span>
</div>

???

* here is result!
* left:
    * software rendering of `triangle.svg`
* right:
    * photograph of `triangle.svg` drawn by pen plotter w/ TODO pen/marker
* we said "thick, blue lines + fill pink"
    * software renderer drew them
    * pen plotter can't
        * just moves pen along paths

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

* now can generate / plot SVGs --> exercise for myself
* how many ways can I fill a rectangle?
    * parameterized by a "darkness" value
        * where 0 ~= empty
        * and 1 ~= completely filled
* how many impls of this trait?
    * take look at few results

---

<img class="invert-90" alt="Rectangle 8" src="images/rectangle-8.svg"/>

???

* *Rectangle 8*
* darkness values = 0.1, 0.5, 0.9
* algorithm:
    * moves distance d along rectangle's outer edges
    * draw lines between points --> creates new quadrilateral
    * continue recursively

---

<img class="invert-90" alt="Rectangle 2" src="images/rectangle-2.svg"/>

???

* *Rectangle 2*
* algorithm:
    * choose random point in rectangle
    * draw boxes out from that point, at uniform distances, until rectangle filled
    * lower darkness value --> fewer boxes
    * higher darkness value --> more boxes

---

exclude: true

<img class="invert-90" alt="Rectangle 6" src="images/rectangle-6.svg"/>

???

* *Rectangle 6*
    * similar to rectangle 2, except start in middle
* boxes closer to the middle = more rounded
* boxes further from middle = less rounded

---

```
trait Tiling {
    fn new(
        rng: &mut impl Rng,
        columns: usize,
        rows: usize,
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

* have many rectangles --> compose via tiling
    * vary darkness value for each rectangle

---

<img class="invert-90 centermiddle" alt="Tiling 0 of Rectangle 10" src="images/tiling-0-rectangle-10.png"/>
<!-- <img class="invert-90 centermiddle" alt="Tiling 0 of Rectangle 10" src="images/tiling-0-rectangle-10.svg"/> -->

???

* *Tiling 0* of *Rectangle 10*
* darkness value = row height
    * first row = 0
    * last row = 1
    * vertical gradient effect

---

<img class="invert-90 centermiddle" alt="Tiling 3 of Rectangle 2" src="images/tiling-3-rectangle-2.png"/>
<!-- <img class="invert-90 centermiddle" alt="Tiling 3 of Rectangle 2" src="images/tiling-3-rectangle-2.svg"/> -->

???

* *Tiling 3* of *Rectangle 2*
* darkness = how close to center of tiling?
    * w/ a little random noise
* I like:
    * kaleidoscope effect
    * stained glass
    * maybe water ripples
    * _emergent_

---

<img class="centermiddle" alt="Tiling 3 of rectangle 2, plotted" src="images/tiling-3-rectangle-2.jpeg"/>

???

* favorite of tiling exercise --> decided to plot it
* another surprise:
    * lines thicker --> much darker than anticipated
* more ghostly

---

# .green[Constraint Breeds Creativity]

### 1. How many ways can I \_\_\_\_\_\_\_\_\_\_\_\_?

### 2. How many ways can I combine those?

???

* hardest part of starting = blank canvas
    * side step w/ constraints + exercise
* write list first, code after?
* how many ways can I...
    * fill rectangle?
    * draw custom brush stroke along given path?
    * visualize Perlin noise?
        * generates smooth gradients instead of pure random
    * different kinds of random walker?
        * move randomly thru coordinate space in interesting ways
* next: how many ways can I combine them?
    * tile rectangles + assign darkness values?
    * draw random walker's paths with custom brush strokes?

---

# .blue[Simple Combinations<br/>of Simple Primitives]

### We don't need a framework to start making plotter art

???

* doesn't take much to start making plotter art
    * don't have to learn framework
    * can start *now*!
* first pieces w/ just `svg` crate
    * SVG is pretty rich
* learning new frameworks can be intimidating
    * large API surface
    * sometimes best way to start = hit the ground running
* once you have a problem that a framework solves,
    * *then* reach for framework

---

<div class="vbox centermiddle" style="width: 65%">
  <div class="hbox flex-1">
    <img class="small-padding flex-1" alt="Software rendering of triangle.svg" src="images/triangle.svg"/>
    <div class="small-padding flex-1">TODO: plot triangle.svg</div>
  </div>
  <div class="hbox flex-1">
    <img class="invert-90 small-padding flex-1" alt="Software rendering of cross-hatch.svg" src="images/cross-hatch.svg"/>
    <div class="small-padding flex-1">TODO: plot cross-hatch.svg</div>
  </div>
</div>

???

* as we saw w/ `triangle.svg`:
    * plotters don't support all SVG features that software renderers do
* software renderers:
    * fill in shapes with colors or gradients
* w/ plotter:
    * need to create cross-hatch pattern

---

<div class="vbox centermiddle" style="width: 65%">
  <div class="hbox flex-1">
    <img class="invert-90 small-padding flex-1" alt="Software rendering " src="images/bad-occlusion.svg"/>
    <div class="small-padding flex-1">TODO: plot bad-occlusion.svg</div>
  </div>
  <div class="hbox flex-1">
    <img class="invert-90 small-padding flex-1" alt="Software rendering of cross-hatch.svg" src="images/good-occlusion.svg"/>
    <div class="small-padding flex-1">TODO: plot good-occlusion.svg</div>
  </div>
</div>

???

* software renderers draw elements in order
    * later elements can occlude earlier elements
    * w/ plotter:
        * draw first element
        * then draw second element,
        * result = elements drawn on top of each other
    * if want occlusion:
        * only generate paths that aren't covered
* running into these problems?
    * consider library / framework

---

# “fitzgen's art”

???

* or make your own personal framework!
    * started collecting my own crates for computational geometry and vector
      graphics
    * named it "fitzgen's art"
* too bloated
    * so to release some of the pressure...

---

# “.green[f].opacity-half[itzgen's] .green[art]” ➟ “.green[fart]”

???

* it became "fart"
    * hence the "flatulence" part of this talk's title
* promise: only fart joke in this talk
    *  nothing else is going to slip out

---

<img class="centermiddle"
     alt="Aspirin Crystals by Carol Roullard"
     src="images/carol-roullard-aspirin-2019.png" />

.headnote[Source: [*Aspirin Crystals* by Carol Roullard](https://mobile.twitter.com/CRoullardArt/status/1104782729017585664)]

???

* I follow people on twitter who do microscope photography
* this photo: Aspirin Crystals by Carol Roullard
    * outward growth until hits another crystal
        * forms zone
    * lines radiating out from center of each crystal
        * almost like exploding fireworks
        * colorful pinwheel effect
* _quite_ cool --> like to make plotter art reminiscent of this

---

<img class="centermiddle" alt="Voronoi Diagram" src="images/voronoi-diagram.svg"/>

.headnote[Source: [Wikipedia](https://en.wikipedia.org/wiki/File:Euclidean_Voronoi_diagram.svg)]

???

* this reminds me of *Voronoi diagrams:*
    * partition of 2D plane
    * set of seed points
    * colors every point on plane by which seed each point is closest
* result = zones similar to our crystal photo

---

<img class="centermiddle" alt="Animation of Voronoi growth" src="images/voronoi-growth.gif"/>

.headnote[Source: [Wikipedia](ohttps://en.wikipedia.org/wiki/File:Voronoi_growth_euclidean.gif)]

???

* makes sense:
    * can create Voronoi diagram w/ flood fill
    * intuitively similar to crystal growth

---

```
let seeds = random_points(N);
for _ in 0..M {
    let p = random_point();
    let c = find_closest(p, &seeds);
    draw_line(p, c);
}
```

???

* understanding of Voronoi diagrams --> create algorithm for crystal-inspired
  art
* choose `N` random points for seeds
* choose many `M` random points:
    * for each one
        * find closest seed
        * draw line to it
* lines create "exploding fireworks" aesthetic

---


<img class="centermiddle" alt="Art inspired by crystal microscopy. Variation 0. Plotted." src="images/crissals-0-plotted.jpeg"/>

???

* simple algorithm works pretty well!
    * here: plotted on black paper w/ white gel pen
    * can see resemblance to crystals
        * zones
        * fireworks
* difference from algorithm just presented:
    * lines only go part way to seed

---

<div class="hbox">
  <img class="small-padding invert-90" alt="Art inspired by crystal microscopy. Variation 1. Software rendering." src="images/crissals-1.svg"/>
  <img class="small-padding" alt="Art inspired by crystal microscopy. Variation 1. Plotted." src="images/crissals-1-plotted.jpeg"/>
</div>

???

* intention: variation in crystal sizes
    * played w/ giving seeds randomized "gravity" level
    * happy little accident; more like comics explosion
* also, pinwheel effect:
    * via length of lines
    * based on angle of line to seed

---

<img class="centermiddle" alt="Art inspired by crystal microscopy. Variation 2. Plotted." src="images/crissals-2-plotted.jpeg"/>

???

* another variation:
    * only choosing points within a circle

---

<img class="centermiddle" alt="A plot of my crystal microscopy-inspired piece" src="images/crissals-with-colors-plotted.jpeg"/>

???

* final variation
    * uses colors for pinwheel effect
* to plot this:
    * each color = different SVG layer
    * plot one layer at a time
    * manually switch pens between layers

---

exclude: true

<img class="centermiddle"
     alt="Charoite by @micROCKScopica"
     src="images/micROCKScopica-charoite-2018.jpg" />

.headnote[Source: [*Charoite* by @micROCKScopica](https://mobile.twitter.com/micROCKScopica/status/1142428535036006401)]

???

* TODO

---

exclude: true

<img class="centermiddle"
     alt="A rock microscopy-inspired piece."
     src="images/rox.jpeg" />

???

* TODO

---

class: middle, center

# .purple[Copy Something]

???

* "good artists copy; great artists steal"
    * I'm new --> settle for "good"
* lesson: find inspiration in something --> copy it
    * someone else's plotter art
        * vintage 1960s plotter art?
    * or translating something unrelated to computers / plotter into plotter
      medium
        * pattern of cracks on glaze of pottery
        * growth patterns of plants / flowers / leaves

---

# .yellow[Frustration:]
## My feedback loop is .blue[too slow]

???

* but: not only easy successes
* earlier, presented process as:
    * write rust program
    * generate SVG
    * plot SVG
* too simplified:
    * don't write perfect programs on the first try
    * "if it compiles, then it works"
        * not found way to encode aesthetic taste into something borrowck can
          enforce at compile time
* really happening is feedback loop:
    * look at SVGs
    * tweak program
    * look at new SVGs
    * tweak program again
    * etc...
    * until happy with output --> plot it

---

## Edit <br/> Compile <br/> Run <br/> Alt-Tab to Firefox <br/> Refresh <br/> Alt-Tab back to Emacs <br/> Repeat

???

* feedback loop is too slow + involves manual work:
    * edit the sources
    * compile the program
    * if no compilation errors --> run the program
    * if runs OK --> alt-tab to Firefox
    * refresh to see the newest SVG
    * alt-tab back to Emacs
    * repeat
* no timing data
    * estimate 3-4 seconds

---

<cite><a href="https://www.kovach.me/posts/2018-10-13-infrastructure-of-art.html">Tips for Generative Infrastructure and Tooling</a> by Benjamin Kovach:</cite>

> ### Rendering new images on any source change
>
> When I make a change in a file, I expect to see the result of that change as
> soon as possible, without doing anything extra.

???

* luckily, Benjamin article: generative art tooling + infra
    * how to solve this problem (and others)
* file watcher --> auto recompile + rerun on change
* if re-compiles OK && generates SVG OK then
    * save `images/<timestamp>.svg`
    * symlink `images/latest.svg`
    * git commit everything: images + code
        * never lose any image
        * have snapshot of code that generated each image
            * riffing on X now
            * later go back and riff on Y
            * exactly what pottery can't do

---

## Edit <br/> Compile <sup>(automatic)</sup> <br/> Run <sup>(automatic)</sup> <br/> Alt-Tab to Firefox <br/> Refresh <br/> Alt-Tab back to Emacs <br/> Repeat

???

* impl'd by adding command-line tool to `fart` framework
    * did speed up feedback loop a *little bit*
* _however:_didn't solve problem
    * feedback loop is _still_ too slow
    * not manually kicking off compiles / running program anymore
    * but still
        * _waiting_ on compiles,
            * in language known for slow compiles
        * alt-tab'ing to Firefox
        * refreshing to see latest SVG
        * alt-tab'ing back to Emacs

---

class: center

## Types of Changes

<img class="invert-90" alt="Graph of types of changes" src="images/types-of-changes.svg"/>

???

* random sample of commits --> categorize changes
    * "algorithmic" changes
    * "constant" changes
    * "formatting" changes
* most of time waiting on tweaking *constants?!*
    * already knew deep down,
    * but having empirical evidence
    * blows my mind

---

```
fart::user_const! {
    const NUMBER_OF_PARTICLES: usize = 1234;
}
```

???

* wanted:
    * change constant values between runs
    * w/out re-compiling
* invented brand new thing: "user constants"
    * might know by name "environment variables"

---

<pre><code class="text remark-code"><span class="purple">$ fart serve</span>
Serving on <span class="yellow">http://localhost:9090</span>
</code></pre>

???

* but "user constants" aren't _just_ environment variables
* local server that goes with them

---

<img class="centermiddle" alt="The fart Web UI in action" src="images/fart-web-ui.png"/>

???

* the local server has a web UI
* for each user constant program uses:
    * form input dynamically created
    * these are over on the bottom left
* tweak the constant --> POST request to the local server
* which updates the environment variable
* re-runs the program
* pushes generated SVG back to web UI
    * preview refreshed automatically

---

<img class="centermiddle" alt="The fart Web UI side by side with Emacs" src="images/fart-in-action.png"/>

???

* additionally: split screen w/ emacs
    * simple, effective: "window management"
    * edit code --> live preview
    * no more alt-tab'ing, even for algorithmic changes

---

## .green[Edit] <br/> .strike[Compile] <br/> .green[Run <sup>(automatic)</sup>] <br/> .strike[Alt-Tab to Firefox] <br/> .strike[Refresh] <br/> .strike[Alt-Tab back to Emacs] <br/> .green[Repeat]

???

* for constant tweaking, ~60% of changes, critical path:
    * edit constant
    * look at updated image
* technically, need re-run
    * but happens automatically
    * not waiting around for run to finish
        * prev screenshot: < 10 ms
        * barely perceptible
* feedback loop is finally near-instantaneous

---

<img class="centermiddle" alt="Crumpled ribbons" src="images/crumpled-ribbons.jpeg"/>

???

* result:
    * explore more ideas
    * faster than previously could
* making new pieces
    * increasingly happy with
    * Crumpled Ribbons
* end of day:
    * hobby != work
    * just for me,
    * having fun making algorithmic art
        * w/ Rust
        * bringing into physical world w/ pen plotter

---

# .yellow[Impose Constraints]
# .purple[Copy Something]
# .green[Make Your Own Tools]

???

* hope story inspired
* take aways generally applicable
    * not just art; also professional programming
* define constraints
    * make problem smaller
    * easier to solve
    * break down into pieces
    * deadline?
* want to learn something?
    * copy it
    * cross pollinate by copying from another domain
* want to boost efficiency?
    * treat iteration time as profiling / optimization problem
    * gather empirical evidence
    * build custom tooling

---

# .green[Learn More]

#### Follow

* [#plottertwitter](https://twitter.com/hashtag/plottertwitter)

#### Watch

* [*Early Plotter Art, 1960s&mdash;1970s*](https://www.youtube.com/watch?v=OR_TzMFhv50) by Sher Minn Chong ([slides](http://piratefsh.github.io/presentations/plotter-history/))
* [*Process Compendium*](https://vimeo.com/22955812) by Casey Reas

#### Read

* [*Computational Drawing*](http://lostritto.com/book) by Carl Lostritto
* [*Computational Geometry in C*](http://cs.smith.edu/~jorourke/books/compgeom.html) by Joseph O'Rourke
* [*Executing Generative Art with a Plotter*](https://tylerxhobbs.com/essays/2018/executing-generative-art-with-a-plotter) by Tyler Hobbs
* [*On Generative Algorithms*](https://inconvergent.net/generative) by Anders Hoff
* [*The Nature of Code*](https://natureofcode.com/) by Daniel Shiffman

???

* few resources I've found helpful
* probably missing a bunch
    * won't fit
    * forgot some
* peruse on your own time

---

<h1 class="purple"><big><big><big><big>Thank You!!</big></big></big></big></h1>
<h3 class="green">Nick Fitzgerald <br/> <a href="https://twitter.com/fitzgen">@fitzgen</a></h3>
