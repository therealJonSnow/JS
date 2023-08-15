<template>
  <canvas ref="canvas" class="fixed top-0 left-0 -z-10" id="canvas"></canvas>
</template>

<script setup>
const props = defineProps(['dotColor'])
const canvas = ref(null)
onMounted(() => {
  
  console.log(props)
  setTimeout(() => {
  // console.log(
  //     '+-+-+-+-+-+-+-+ +-+-+-+ +-+ +-+-+-+-+-+-+-+-+-+-+\n' +
  //           '|L|o|o|k|i|n|g| |f|o|r| |a| |d|e|v|e|l|o|p|e|r|:|\n' +
  //           '+-+-+-+-+-+-+-+ +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+\n' +
  //           '|W|h|y| |n|o|t| |c|o|n|t|a|c|t| |m|e|?|\n' +
  //           '+-+-+-+ +-+-+-+ +-+-+-+-+-+-+-+ +-+-+-+\n'
  //   )
    // Canvas
    const c = canvas.value;
    const ctx = c.getContext('2d');

    // Variables
    let [cw, ch] = [window.innerWidth, window.innerHeight];
    let [lastX, lastY] = [null, null];
    let dots = [];
    let resizeTimer;

    // Settings
    let [DOT_SMALL, DOT_LARGE] = [1, 20];
    const HOVER_RADIUS = 250;
    const DOT_DECAY = 0.15;
    let [NUM_ROWS, NUM_COLS] = [Math.ceil(ch/DOT_LARGE), Math.ceil(cw/DOT_LARGE)];
    let NUM_DOTS = NUM_ROWS*NUM_COLS;

    // requestAnimationFrame fallback
    window.requestAnimFrame = (function() {
      return  window.requestAnimationFrame       ||
              window.webkitRequestAnimationFrame ||
              window.mozRequestAnimationFrame    ||
              window.msRequestAnimationFrame     ||
              function( callback ){
                  window.setTimeout(callback, 1000 / 60);
              };
    })();


    //--------------------------------------------------------------------


    // Create a new dot
    function Dot(radius, x, y, hue) {
      this.radius = radius;
      this.x = x;
      this.y = y;
      this.hue = hue;
      this.inHoverRadius = false;
    }

    // Update a dot
    Dot.prototype.update = function() {
      // Draw the dot
      ctx.beginPath();
      ctx.arc(
        this.x,
        this.y,
        this.radius,
        0,
        Math.PI * 2,
        false
      );
      ctx.closePath();
  
      // Style it up
  //     ctx.fillStyle = `hsla(${this.hue}, 100%, 50%, 1)`;
        ctx.fillStyle = props.dotColor;


      ctx.fill();

      // Check the mouses proximity
      this.checkProximity();

      // Decay the radius
      this.decayRadius();
    };

    // Check the dots proximity
    Dot.prototype.checkProximity = function() {
      let dist = DOT_SMALL;

      // Check if the mouse is on the canvas
      if (lastX && lastY) {
        // Get proximity
        let dX = this.x - lastX;
        let dY = this.y - lastY;
        dist = Math.sqrt(Math.pow(dX, 2) + Math.pow(dY, 2));

        // Update dot radius
        if (dist >= HOVER_RADIUS) {
          this.radius = this.radius;
          this.inHoverRadius = false;
        } else {
          // this.radius = DOT_SMALL/2 + ((DOT_LARGE*2) - (DOT_LARGE*(dist/100)));
          // this.radius = DOT_SMALL/2 + (DOT_LARGE*((HOVER_RADIUS-dist)/10));
          // this.radius = DOT_SMALL/2 + (DOT_LARGE - (DOT_LARGE*dist/HOVER_RADIUS));
          // this.radius = DOT_SMALL/2 + (DOT_LARGE - (DOT_LARGE*dist/HOVER_RADIUS));
          this.radius = Math.max(DOT_SMALL, Math.min(DOT_SMALL * 1.5, (1 / dist) * 150))
          this.inHoverRadius = true;
        }
      } else {
        this.inHoverRadius = false;
      }
    }

    // Check the dots radius decay
    Dot.prototype.decayRadius = function() {
      if (!this.inHoverRadius && this.radius > DOT_SMALL) {
        this.radius = this.radius - DOT_DECAY;
      }
    }

    // Add a new dot
    function addDot(radius, x, y, hue) {
      radius = radius || DOT_SMALL/2;
      x = x;
      y = y;
      hue = hue || Math.floor(Math.random() * (360 - 1 + 1)) + 1;

      // Create the new dot
      let dot = new Dot(radius, x, y, hue);

      // Add the dot to the array
      dots.push(dot);
    }

    // Add our initial dots
    function init() {
      dots = [];
      
      // Add the dots
      for (let row = DOT_LARGE; row <= ch; row += DOT_LARGE*2) {
        for (let col = DOT_LARGE; col <= cw; col += DOT_LARGE*2) {
          addDot(DOT_SMALL, col, row);
        }
      }
      
      draw();
    }

    // Clear the canvas and draw the new frame
    function draw() {
      ctx.clearRect(0, 0, cw, ch);

      // Update the dots
      for (let i = 0; i < dots.length; i++) {
        dots[i].update();
      }
      
      // Add helper text
      // ctx.fillStyle = 'black';
      // ctx.textAlign='center'; 
      // ctx.font = '12px Arial';
      // ctx.fillText('Move you mouse around and watch the dots change.', (cw / 2), 15);

      requestAnimFrame(draw);
    }

    // Update the size of the canvas
    function resizeCanvas() {
      // console.log(window)
      [cw, ch] = [window.innerWidth, window.innerHeight];
      
      // Update canvas size
      [c.width, c.height] = [cw, ch];
      
      // Update rows, cols, and number of dots
      [NUM_ROWS, NUM_COLS] = [Math.ceil(ch/(DOT_LARGE*2))+1, Math.ceil(cw/(DOT_LARGE*2))];
      NUM_DOTS = NUM_ROWS*NUM_COLS;

      clearTimeout(resizeTimer);
      resizeTimer = setTimeout(function() {
        init();
      }, 250);
    }


    //--------------------------------------------------------------------


    // Set mouse coordinates
    window.addEventListener('mousemove', (e) => {
      [lastX, lastY] = [e.clientX, e.clientY];
    }, false);

    window.addEventListener('mouseout', (e) => {
      [lastX, lastY] = [null, null];
    }, false);

    // Click events
//     c.addEventListener('click', (e) => {
//       // Update the dots
//       for (let i = 0; i < dots.length; i++) {
//         dots[i].radius = DOT_LARGE/2;
//       }
//     }, false);

    // Window resize
    window.addEventListener('resize', resizeCanvas, false);


    //--------------------------------------------------------------------


    // Initialize the fun
    resizeCanvas();
    init();

  }, 1) // 1 seems to work better for me than 0
})
</script>
<style>
</style>