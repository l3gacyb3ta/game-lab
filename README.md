# 👾 Hack Club Game Lab

**[Launch the Game Lab →](https://game-lab.hackclub.dev/)**

The Hack Club Game Lab is a microworld to discover programming and the joys of making things you want with computers by making simple games. It's Hack Club's version of a [fantasy console](https://en.wikipedia.org/wiki/Fantasy_video_game_console) (ex. [PICO-8](https://www.lexaloffle.com/pico-8.php)), and is designed to help run engaging Hack Club meetings by making it easy to get started building things instead of explaining things!

The game lab is designed with the principles of [constuctionism](https://en.wikipedia.org/wiki/Constructionism_(learning_theory)). It's the idea that people learn best when they make things that they care about which they can share with others. We made Game Lab so you can learn programming by building programs filled with character and delight. 

Design goals:

- A new coder without any prior coding experience should be able to figure out how to build a game like Pong within an hour from looking at examples and tinkering
- New coders will generate ideas for their own games they feel they can build with Game Lab through looking at examples
- Experienced coders should have fun making games in Game Lab too!

Join `#gamelab-dev` on the [Hack Club Slack](https://hackclub.com/slack/) to join the develpoment discussion!

<img width="500" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149387903-eec65489-6a8d-4779-adde-2b6e35c7273a.gif">

Check out a minimal example game: [Superb Macedonian Plumber Bro](https://game-lab.hackclub.dev/?file=recxJ4Z4C3U0WbSIT).

## Development

The Hack Club Game Lab requires a local HTTP server to run in development. Here's how to get it running on your own machine.

Clone repo:

    $ git clone https://github.com/hackclub/game-lab

Start a local HTTP server inside the repo:

    $ cd game-lab/
    $ python3 -m http.server 3000

And then go to http://localhost:3000 in your web browser, and it should work!

## Versions

Game-Lab is in active development. We want to make sure you can play the games you make even if they aren't compatible with the newest version of the editor. If you made a game and need to run it on an old version of Game-Lab you can use this site: https://game-lab-server-1.maxwofford.repl.co/[SEMANTIC_VERSION]/index.html

For example the first release of Game-Lab is available here: https://game-lab-server-1.maxwofford.repl.co/0.1.0/index.html

## Minimal Examples

Initialize the Game Engine:

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);
```

---

[Make a character:](https://game-lab.hackclub.dev/?file=recUlLLmfEksmALAM)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149362983-6f82a61c-c3d5-40b7-920f-f673c3ff2646.png">

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);

e.add({
  tags: ["test"],
  x: 188,
  y: 69,
  sprite: test_sprite,
  scale: 4,
})

e.start();
```

---

[Add a floor with gravity:](https://game-lab.hackclub.dev/?file=recMIYxXueSz8tXQs)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149363706-453a45e8-d0d4-44a3-acc3-09e4bb577392.gif">

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);

e.add({
  tags: ["test"],
  solid: true,
  x: 135,
  y: 114,
  sprite: test_sprite,
  scale: 4,
  update(obj) {
    obj.vy += 2;
  }
})

e.add({
  tags: ["floor"],
  solid: true,
  x: -6,
  y: 283,
  sprite: floor,
  scale: 11,
})

e.start();
```

---

[Add movement:](https://game-lab.hackclub.dev/?file=rec7bKM7Iiq7f6vFF)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149365452-7b042996-2beb-40a8-866e-f5748b5631da.gif">

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);

e.add({
  tags: ["test"],
  solid: true,
  x: 135,
  y: 114,
  sprite: test_sprite,
  scale: 4,
  update(obj) {
    obj.vy += 2;
  }
})

e.add({
  tags: ["floor"],
  solid: true,
  x: -6,
  y: 283,
  sprite: floor,
  scale: 11,
})

e.start();
```

---

[Add jump:](https://game-lab.hackclub.dev/?file=rec75TEodfPebIzlc)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149366181-588ae196-03dd-4268-9907-9477caa8a834.gif">

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);

e.add({
  tags: ["test"],
  solid: true,
  x: 135,
  y: 114,
  sprite: test_sprite,
  scale: 4,
  collides(me, them) {
    if (e.pressedKey(" ")) {
      if (them.hasTag("floor")) me.vy -= 19;
    }
  },
  update(obj) {
    obj.vy += 2;

    if (e.heldKey("ArrowLeft")) obj.x -= 3;
    if (e.heldKey("ArrowRight")) obj.x += 3;
  }
})

e.add({
  tags: ["floor"],
  solid: true,
  x: -6,
  y: 283,
  sprite: floor,
  scale: 11,
})

e.start();
```

---

[Add platforms:](https://game-lab.hackclub.dev/?file=recBW6ApDr7xzlhxz)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149367516-7edb2780-edbd-4977-9a07-dcfbf47fcf93.gif">

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);
const ctx = e.ctx;

e.add({
  tags: ["player"],
  sprite: test_sprite,
  scale: 3,
  solid: true,
  x: 50,
  y: 16,
  collides(me, them) {
    if (them.hasTag("platform")) me.vx = them.vx;
    
    if (e.pressedKey(" ")) {
      if (them.hasTag("platform")) me.vy -= 11;
    }
  },
  update: (obj) => {
      obj.ay = 0.4;
  
      if (e.heldKey("ArrowLeft")) obj.x -= 3;
      if (e.heldKey("ArrowRight")) obj.x += 3;
  },
})

const addPlatform = (x, y) => e.add({
  tags: ["platform"],
  sprite: floor,
  scale: 3,
  solid: true,
  x: x,
  y: y,
  vx: -1,
  bounce: -1,
  update: (obj) => {
      if (obj.x < 0) obj.vx = 1;
      if (obj.x + obj.width > e.width) obj.vx = -1
  },
})

addPlatform(50, 200);
addPlatform(20, 100);

e.start();
```

---

[Add a background:](https://game-lab.hackclub.dev/?file=recEy698LbPNQlL1G)

<img width="333" alt="Screen Shot 2022-01-13 at 11 21 43 AM" src="https://user-images.githubusercontent.com/27078897/149368356-c343a214-0d31-4d5f-a2d4-d0575b18047b.png">

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);

e.add({
  update() {
    e.ctx.fillStyle = "pink";
    e.ctx.fillRect(0, 0, e.width, e.height);
  }
})

e.start();
```

---

[Add collisions](https://game-lab.hackclub.dev/?file=recC0NfXt8DIWQU3H)

<img width="333" alt="Screen Shot 2022-01-13 at 11 21 43 AM" src="https://user-images.githubusercontent.com/27078897/149369879-7d384b3a-2f15-4816-a59e-76b56bb9a944.gif">

```js
const canvas = document.querySelector(".game-canvas");
const e = createEngine(canvas, 300, 300);
const ctx = e.ctx;

e.add({
  tags: ["player"],
  sprite: test_sprite,
  scale: 2,
  x: 150,
  y: 50,
  update: (obj) => {
    if (e.heldKey("ArrowUp")) obj.y -= 3;
    if (e.heldKey("ArrowDown")) obj.y += 3;
  },
})

e.add({
  tags: ["target"],
  sprite: floor,
  scale: 3,
  x: 112,
  y: 232,
  collides(me, them) {
    if (them.hasTag("player")) e.remove("target");
  }
})

e.start();
```

---

Refer to the following example for all the available object properties:

```js
e.add({
  tags: ["tag-name"],
  solid: true,
  x: 178,
  y: 126,
  vx: 1,
  vy: 3,
  ax: 0,
  ay: 0,
  sprite: ball,
  scale: 2,
  bounce: 1,
  origin: [0, 0], // x: 0 - 1, y: 0 - 1, "left|center|right top|center|bottom", "center"
  collides(me, them) {
    if (them.hasTag("tag-name")) {
      me.vx *= 1.3;
      me.vy *= 1.3;
    }
  },
  update: (obj) => {
    if (e.heldKey("ArrowDown")) obj.y += 3;
    if (e.pressedKey("ArrowUp")) obj.y -= 3;

    if (obj.y < 0) obj.vy *= -1;
    if (obj.y > e.height - obj.height) obj.vy *= -1;
    if (obj.x < 0) {
      e.end();
      e.addText("Blue Wins", e.width/2, e.height/2, { color: "blue", size: 32 })
    }

    if (obj.x > e.width - obj.width) {
      e.end();
      e.addText("Red Wins", e.width/2, e.height/2, { color: "red", size: 32 })
    }
  },
})
```

## Tiny Games

[Pong-ish](https://game-lab.hackclub.dev/?file=recZMaBjgnNXgZsUK)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149371012-faf3e45f-9d3a-47d4-831b-566d9171d2bd.gif">

---

[Crappy Birds](https://game-lab.hackclub.dev/?file=recJX7dAboz7v1OFG)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/149380918-a1855ab3-cc2d-4a9a-adc0-d5316d6f17ba.gif">

---

[Brick Broken](https://game-lab.hackclub.dev/?file=rec6bdF7IS7vY7xzl)

<img width="345" alt="Screen Shot 2022-01-13 at 10 50 41 AM" src="https://user-images.githubusercontent.com/27078897/150606449-5b73d7fe-f2d3-432f-9cc5-346c20919ec8.gif">


## License

The Hack Club Game Lab is open source and licensed under the [MIT License](./LICENSE). Fork, remix, and make it your own! Pull requests and other contributions greatly appreciated.
