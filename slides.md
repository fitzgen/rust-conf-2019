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
* back up first and tell a story

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
