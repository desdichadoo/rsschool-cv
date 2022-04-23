<header>CV</header>
<h1>Alexander Smoev</h1>
###### Navigation
<nav>
  <ul>
    <li><a href="#contact-info">Contact info</a></li>
    <li><a href="#summary">Summary</a></li>
    <li><a href="#skills">Skills</a></li>
    <li><a href="#code-examples">Code Examples</a></li>
    <li><a href="#experience">Experience</a></li>
    <li><a href="#education">Education</a></li>
    <li><a href="#english">English</a></li>
  </ul>
</nav>

  <h2 id = "contact-info">Contact info</h2>
  <ul>
  <li><a href="https://www.facebook.com/alexander.smoev/">Facebook</a></li>
  <li><a href="https://www.linkedin.com/in/alex-smoev-a51665188/">LinkedIn</a></li>
  </ul>
  <h2 id = "summary">Summary</h2>
For now, my goal is to learn programming because I really liked it while studying Bitcamp so I tried to give it a shot. If you ask more specific evaluation of the goal, I don't know what it will be - front-end, back-end or middle-end (high IQ joke especially for you dear reader), but we'll soon get to the answer.
  <h2 id = "skills">Skills</h2>
From skills, I can proudly say I've finished Bitcamp's first stage, so I know javascript a little bit. Also, I studied HTML and also kinda know it. That's it, I'm a guy that kinda knows some stuff. Also I've got other skills, so to summarize:
  <ul>
    <li>Javascript</li>
    <li>HTML</li>
    <li>Hard-working</li>
  </ul>
  <h2 id = "code-examples">Code Examples</h2>
  <i>You should divide the canvas into an imaginary grid with NUM_RECTANGLES_ACROSS rectangles across, and NUM_RECTANGLES_DOWN rectangles down. Each time the user moves the mouse, a rectangle aligned with this grid should be drawn so that the mouse’s location is within the rectangle. The rectangle should change color each time the mouse passes over it.

This requires using the mouseMoveMethod as well as writing a function:</i>

```js 
var NUM_RECTANGLES_ACROSS = 4;
var NUM_RECTANGLES_DOWN = 10;
var x = -getWidth()/4;
var y = -getHeight()/10;
var search;

function start(){
    mouseMoveMethod(putBlinkingRectangles);
}

function putBlinkingRectangles(e){
    for(var i =0; i <= 4; i++){
        if((e.getX() <= getWidth()/4 * i) && (e.getX() >= getWidth()/4 * (i-1))){
            x = getWidth()/4 * (i-1);
        }
    }
    for(var i =0; i <= 10; i++){
        if((e.getY() <= getHeight()/10 * i) && (e.getY() >= getHeight()/10 * (i-1))){
            y = getHeight()/10 * (i-1);
        }
    }
    var rect = new Rectangle(getWidth()/NUM_RECTANGLES_ACROSS, getHeight()/NUM_RECTANGLES_DOWN);
    rect.setPosition(x,y);
    rect.setColor(Randomizer.nextColor());
    add(rect);
}
```
  <i>This is final project on Bitcamp and I chose to create a simple paddle game in which you'll never win and only lose (yeah, it's a long one...):</i>

```js
var ball;
var dx = 5;
var dy = 5;
var radius = 15;
var paddle1;
var paddle2;
var paddleX;
var count = 1;
var txt;
var ballsLost = 0;
var search;

function start(){
    textBox("Click to continue, but I warn You...", "20pt Calibri", Color.red, 15, 150);
    loseWin();
    drawPaddle1();
    drawPaddle2();
    if(ballsLost == 3){
        null;
    }else{
        mouseMoveMethod(movePaddle1);
    }
    mouseClickMethod(startTheGame);
}

function drawBall(radius, color, x, y){
    ball = new Circle(radius);
    ball.setPosition(x, y);
    ball.setColor(color);
    add(ball);
}

function draw(){
    remove(txt);
    textBox("You have " + (3 - ballsLost) + " lives left", "12pt Calibri", Color.black, 0, 10);
    ball.move(dx, dy);
    if(ball.getX() + radius >= getWidth()){
        dx = -dx;
    }
    if(ball.getX() - radius <= 0){
        dx = -dx;
    }
    if(ball.getY() + radius >= getHeight()){
        remove(ball);
        remove(txt);
        stopTimer(draw);
        ballsLost++;
        loseWin();
        dy = 5;
    }
    if(ball.getY() - radius <= 0){
        remove(ball);
        remove(txt);
        stopTimer(draw);
        loseWin();
        dy = 5;
        println("The red guy has infinite lives buddy");
    }
    search = getElementAt(ball.getX(), ball.getY() - radius);
    if(search != null){
        dy = -dy;
    }
    search = getElementAt(ball.getX(), ball.getY() + radius);
    if(search != null){
        dy = -dy - 1;
    }
    search = getElementAt(ball.getX() + radius, ball.getY());
    if(search != null){
        dx = -dx - 1;
    }
    search = getElementAt(ball.getX() -radius, ball.getY());
    if(search != null){
        dx = -dx;
    }
    if(ball.getY() >= getHeight()/2){
        ball.setColor(Color.green);
    }else{
        ball.setColor(Color.red);
    }
    if(ballsLost == 3){
        stopTimer(draw);
        removeAll();
        textBox("Congratulations, You lost", "20pt Calibri", Color.red, 65, getHeight()/2);
    }
}

function loseWin(){
    drawBall(radius, Color.black, getWidth()/2, getHeight()/2);
    count --;
}

function startTheGame(e){
    count++;
    if(count % 2 == 0){
        setTimer(draw, 10);
        setTimer(drawPaddle2Movement, 1);
    }else{
        count--;
    }
    remove(txt);
    if(count <= 1){
        count++;
        textBox("You'll never win this game", "20pt Calibri", Color.red, 60, 150);
    }
}

function drawPaddle1(){
    paddle1 = new Rectangle(80, 10);
    paddle1.setPosition(40, getHeight() - 10 * 2);
    paddle1.setColor(Color.green);
    add(paddle1);
}

function drawPaddle2(){
    paddle2 = new Rectangle(80, 10);
    paddle2.setPosition(40, 15);
    paddle2.setColor(Color.red);
    add(paddle2);
}

function textBox(label, font, color, x, y){
    txt = new Text(label, font);
    txt.setPosition(x, y);
    txt.setColor(color);
    add(txt);
}

function drawPaddle2Movement(){
    if(ball.getX() - 80/2 <= 0){
        paddleX = 0;
    }else{
        if(ball.getX() + 80/2 >= getWidth()){
            paddleX = getWidth() - 80;
        }else{
            paddleX = ball.getX() - 80/2;
        }
    }
    paddle2.setPosition(paddleX, 10);
}

function movePaddle1(e){
    if(e.getX() - 80/2 <= 0){
        paddleX = 0;
    }else{
        if(e.getX() + 80/2 >= getWidth()){
            paddleX = getWidth() - 80;
        }else{
            paddleX = e.getX() - 80/2;
        }
    }
    paddle1.setPosition(paddleX, getHeight() - 20);
}
```
  <h2 id = "experience">Experience</h2>
I have a little experience in Bitcamp, I've finished first stage. Also, I finished a course in FreeUni on basics of programming.
  <h2 id = "education">Education</h2>
I'm studying in FreeUni on ESM, but also am interested in coding.
  <h2 id = "english">English</h2>
English level of C1. I've studied business English in university and can speak it fluently.

<footer> 
  <a href="https://gist.github.com/desdichadoo">github</a> 23 April 2022
  <p>
    <a href="https://rs.school/js-en"/><img border="0" alt="Javascript/Front-end Mentoring Program (in English)" src="https://rs.school/images/rs_school_js.svg" class="center" style="width:50%;"><a/>
  </p>
</footer>
