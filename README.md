# BINARY RECREATION BY LEXUS, DEONDRE AND MAHAD

```
// module aliases
var Engine = Matter.Engine,
	Render = Matter.Render,
	Runner = Matter.Runner,
	Events = Matter.Events,
	Composite = Matter.Composite,
	Composites = Matter.Composites,
	Common = Matter.Common,
	MouseConstraint = Matter.MouseConstraint,
	Mouse = Matter.Mouse,
	Composite = Matter.Composite,
	Vector = Matter.Vector,
	Vertices = Matter.Vertices,
	Bounds = Matter.Bounds,
	Body = Matter.Body,
	Bodies = Matter.Bodies;

let mouseConstraint
let engine, render

// Pipe
let pipe;

// Background Color
let bgColour = 'orange'

// The numbers
let n1 = 0,
	n2 = 0,
	n3 = 0,
	n4 = 0,
	n5 = 0,
	n6 = 0,
	n7 = 0,
	n8 = 0,
	n9 = 0;

// Binary digits variables
let binary0 = 0,
	binary1 = 0,
	binary2 = 0,
	binary3 = 0,
	binary4 = 0,
	binary5 = 0,
	binary6 = 0,
	binary7 = 0,
	binary8 = 0;


// Score and timer variables
let Score = 0
let ScoreLastChecked = 0
let countdown = 300
let countdownLastCheckedTime = 100
let State = 0


// Tab variables
let tabHeight = 0
let tabWidth = 0
let tabX = 0
let tabY = 0
let tabs = []

// Button variables
let ballSpawnX = 0
let ballSpawnY = 0
let ButtonX = 0
let ButtonY = 0
let normalrollbuttonX = 0
let normalrollbuttonY = 0
let epicrollbuttonX = 0
let epicrollbuttonY = 0
let r = 30
let count = 0
let Buttons = []

// Ball variables
let ballRadius = 0
let ballX = 0
let ballY = 0

// RNG Variables
let rng1 = 0
let rng2 = 0
let rng3 = 0
let rng4 = 0

// Answer
let answer = 0
let sum = 0

// Question difficulty level
let difficulty = 0
let min = 0
let max = 0

// Rolls
let normalrolls = 0
let epicrolls = 0

// Boolean variables
// Start and Pause variables
let gameStart = false
let gamePause = false

// Discovered item variables. 
// These variables are for checking if an object has been discovered by the player.

function preload() {
	pipe = loadImage('Group 3.png')
}

function setup() {
	MyWorld()
	for (let i = 0; i < 9; i++) {
		ButtonY = tabY + r
		ButtonX = tabX - tabWidth * i
		let buttons = new Button(ButtonX, ButtonY)
		Buttons.push(buttons)
	}
}

function MyWorld() {
	// Creates an engine and a renderer

	engine = Engine.create();
	render = Render.create({
		element: document.body,
		engine: engine,
		options: {
			width: innerWidth,
			height: innerHeight,
			wireframes: false,
			showDebug: false,
			showCollisions: false,
			background: '#2A4974',
		}
	});

	// This lets use draw on the canvas using processing's (non-physics) drawing tools
	createCanvas(windowWidth, windowHeight, render.canvas)



	// Creates a mouse constraint
	var mouse = Mouse.create(render.canvas)
	mouseConstraint = MouseConstraint.create(engine, {
		mouse: mouse,
		constraint: {
			stiffness: 0.2,
			render: {
				visible: false
			}
		}
	});

	// Adds the mouse constraint
	Composite.add(engine.world, mouseConstraint)


	// Creates the walls
	TopWall = Bodies.rectangle(0, 0, width * 2, 50, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});
	BottomWall = Bodies.rectangle(0, height, width * 2, 50, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});
	LeftWall = Bodies.rectangle(0, 0, 50, height * 2, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});
	RightWall = Bodies.rectangle(width, 0, 50, height * 2, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});

	// Tabs
	for (let i = 0; i < 9; i++) {
		tabHeight = 100
		tabY = height / 2
		tabWidth = width / 16
		tabX = LeftWall.position.x + width / 4 + i * tabWidth
		let tab = new Tabs(tabX, tabY)
		tabs.push(tab)
		var tabBody = Bodies.rectangle(tabX, tabY, tabWidth / 2, tabHeight, {
			isStatic: true,
			render: {
				fillStyle: '#8F8AFF',
				strokeStyle: '#FF85CA'
			}
		})
		// Composite.add(engine.world, tabBody)
	}


	// Properties for the testing ball.
	ballRadius = tabWidth / 3
	ballX = 300
	ballY = 300
	// Creates shapes
	testingBall = Bodies.circle(ballX, ballY, ballRadius, {
		restitution: 1,
		render: {}
	})

	// Creates a stack of balls
	var stack = Composites.stack(150, 200, 1, 5, 0, 0, function(x, y) {
		return Bodies.circle(x, y, ballRadius, {
			isStatic: false,
			restitution: 1
		});
	});

	// Adds all the bodies
	// Composite.add(engine.world, [TopWall, BottomWall, RightWall, LeftWall, testingBall])

	// run the renderer
	Render.run(render);

	// create runner
	var runner = Runner.create();

	// run the engine
	Runner.run(runner, engine);
}

function draw() {
	// Menu that appears when the countdown goes all the way down to 0
	gameDone()

	// The game's start menu
	gameStartMenu()

	if (gameStart === true) {
		// RNG Numbers
		// RNG()
		// text(rng1, 100, 200)
		// text(rng2, 100, 300)
		// text(rng3, 100, 400)
		// text(rng4, 100, 500)

		// Binary numbers made by Lexus
		Binary()

		// Timer made by Deondre
		timer()

		// Buttons on the tabs
		TabButtons()

		// The Question/number you have to make with the binary numbers
		Question()

		// draws the pipe
		imageMode(CENTER)
		image(pipe, 100, 185)
		//Button on pipe
		stroke('rgb(75,75,75)')
		fill('rgb(253,82,82)')
		strokeWeight(10)
		circle(100, 165, r*1.5)
		textAlign(LEFT)
		textSize(width/50)
		fill('white')
		text('<--- Serves no purpose yet', 150, 165)
	}
}

function TabButtons(){
	Buttons.forEach(newButtons => {
		newButtons.draw()
	})
}

function Binary() {
	// First binary #
	if (binary0 == 1) {
		n1 = 1
	} else if (binary0 == 0) {
		n1 = 0
	}
	
	// Second binary #
	if (binary1 == 1) {
		n2 = 2
	} else if (binary1 == 0) {
		n2 = 0
	}
	
	// Third binary #
	if (binary2 == 1) {
		n3 = 4
	} else if (binary2 == 0) {
		n3 = 0
	}
	
	// Fourth binary #
	if (binary3 == 1) {
		n4 = 8
	} else if (binary3 == 0) {
		n4 = 0
	}
	
	// Fifth binary #
	if (binary4 == 1) {
		n5 = 16
	} else if (binary4 == 0) {
		n5 = 0
	}
	
	// Sixth binary #
	if (binary5 == 1) {
		n6 = 32
	} else if (binary5 == 0) {
		n6 = 0
	}
	
	// Seventh binary #
	if (binary6 == 1) {
		n7 = 64
	} else if (binary6 == 0) {
		n7 = 0
	}
	
	// Eighth binary #
	if (binary7 == 1) {
		n8 = 128
	} else if (binary7 == 0) {
		n8 = 0
	}
	
	// Ninth binary #
	if (binary8 == 1) {
		n9 = 256
	} else if (binary8 == 0) {
		n9 = 0
	}
	
	textSize(25)
	textAlign(CENTER)
	fill('black')
	noStroke()
	text(binary8, tabX - (tabWidth * 8), tabY)
	text(binary7, tabX - (tabWidth * 7), tabY)
	text(binary6, tabX - (tabWidth * 6), tabY)
	text(binary5, tabX - (tabWidth * 5), tabY)
	text(binary4, tabX - (tabWidth * 4), tabY)
	text(binary3, tabX - (tabWidth * 3), tabY)
	text(binary2, tabX - (tabWidth * 2), tabY)
	text(binary1, tabX - (tabWidth), tabY)
	text(binary0, tabX, tabY)

	sum = n1 + n2 + n3 + n4 + n5 + n6 + n7 + n8 + n9
	text("Sum of all #'s: "+sum, windowWidth / 2, windowHeight / 3)
}

class Tabs {
	constructor(x, y) {
		this.position = createVector(x, y)
	}
}

function TabBodies() {
	// Tabs
	for (let i = 0; i < 9; i++) {
		tabHeight = 100
		tabY = height / 2
		tabWidth = width / 16
		tabX = LeftWall.position.x + width / 4 + i * tabWidth
		let tab = new Tabs(tabX, tabY)
		tabs.push(tab)
		var tabBody = Bodies.rectangle(tabX, tabY, tabWidth / 2, tabHeight, {
			isStatic: true,
			render: {
				fillStyle: '#8F8AFF',
				strokeStyle: '#FF85CA'
			}
		})
		Composite.add(engine.world, tabBody)
	}
}

class Button {
	constructor(x, y) {
		this.x = ButtonX
		this.y = ButtonY
	}
	draw() {
		fill('blue')
		circle(this.x, this.y, r)
	}
}

function mouseClicked() {
	let pipeButton = createVector(100, 165)
	let buttonVector1 = createVector(tabX, tabY + r)
	let buttonVector2 = createVector(tabX - tabWidth * 1, tabY + r)
	let buttonVector3 = createVector(tabX - tabWidth * 2, tabY + r)
	let buttonVector4 = createVector(tabX - tabWidth * 3, tabY + r)
	let buttonVector5 = createVector(tabX - tabWidth * 4, tabY + r)
	let buttonVector6 = createVector(tabX - tabWidth * 5, tabY + r)
	let buttonVector7 = createVector(tabX - tabWidth * 6, tabY + r)
	let buttonVector8 = createVector(tabX - tabWidth * 7, tabY + r)
	let buttonVector9 = createVector(tabX - tabWidth * 8, tabY + r)
	let mouseVector = createVector(mouseX, mouseY)
	let pipeButtondist = pipeButton.dist(mouseVector)
	let distance1 = buttonVector1.dist(mouseVector)
	let distance2 = buttonVector2.dist(mouseVector)
	let distance3 = buttonVector3.dist(mouseVector)
	let distance4 = buttonVector4.dist(mouseVector)
	let distance5 = buttonVector5.dist(mouseVector)
	let distance6 = buttonVector6.dist(mouseVector)
	let distance7 = buttonVector7.dist(mouseVector)
	let distance8 = buttonVector8.dist(mouseVector)
	let distance9 = buttonVector9.dist(mouseVector)

	if (pipeButtondist < r * 1.5 / 2){
		Composite.add(engine.world, [
			Bodies.circle(100, 160, 20, {
				restitution: 1
			})
		])
	}

		if (distance1 < r / 2 && binary0 == 0) {
			binary0 = 1
		} else if (distance1 < r / 2 && binary0 == 1) {
		binary0 = 0
	}
	if (distance2 < r / 2 && binary1 == 0) {
		binary1 = 1
	} else if (distance2 < r / 2 && binary1 == 1) {
		binary1 = 0
	}
	if (distance3 < r / 2 && binary2 == 0) {
		binary2 = 1
	} else if (distance3 < r / 2 && binary2 == 1) {
		binary2 = 0
	}
	if (distance4 < r / 2 && binary3 == 0) {
		binary3 = 1
	} else if (distance4 < r / 2 && binary3 == 1) {
		binary3 = 0
	}
	if (distance5 < r / 2 && binary4 == 0) {
		binary4 = 1
	} else if (distance5 < r / 2 && binary4 == 1) {
		binary4 = 0
	}
	if (distance6 < r / 2 && binary5 == 0) {
		binary5 = 1
	} else if (distance6 < r / 2 && binary5 == 1) {
		binary5 = 0
	}
	if (distance7 < r / 2 && binary6 == 0) {
		binary6 = 1
	} else if (distance7 < r / 2 && binary6 == 1) {
		binary6 = 0
	}
	if (distance8 < r / 2 && binary7 == 0) {
		binary7 = 1
	} else if (distance8 < r / 2 && binary7 == 1) {
		binary7 = 0
	}
	if (distance9 < r / 2 && binary8 == 0) {
		binary8 = 1
	} else if (distance9 < r / 2 && binary8 == 1) {
		binary8 = 0
	}


	gameStartMenu()

	if (gameStart === false && mouseX >= width / 2 - width / 10 && mouseX <= width / 2 + width / 10) {
		gameStart = true
		// Adds all the bodies
		Composite.add(engine.world, [TopWall, BottomWall, RightWall, LeftWall])
		countdown = 300
	}
}

function timer() {
	// Timer coded by Deondre. Edited by Lexus
	rectMode(CENTER)
		// textAlign(CENTER);
		textSize(20);
		fill(255);
		stroke(5);
		strokeWeight(4);
		rect(125, 50, 200, 50)
		rect(100, 100, 150, 50)
		text("score: " + Score, 75, 50)
		text("timer: " + countdown, 75, 100)
	
	
	if (State == 0) {
		let now = millis()
		if (now - ScoreLastChecked > 1000) {
			ScoreLastChecked = now
			let later = millis()
			if (later - countdownLastCheckedTime > 1000)
				countdown -= 1
			if (countdown == 0) {
				countdown -= 0
				State = 1
			}
		}
	}
}

function RNG(){
	if (rng1 <= 0){
	rng1 = (Math.round(random(10)))
	}
	if (rng2 <= 0){
	rng2 = (Math.round(random(10)))
	}
	if (rng3 <= 0){
	rng3 = (Math.round(random(10)))
	}
	if (rng4 <= 0){
	rng4 = (Math.round(random(10)))
	}
}

function gameStartMenu() {
	if (gameStart === false) {
		rectMode(CENTER)
		fill('rgb(183,119,249)')
		stroke('rgb(0,0,0)')
		strokeWeight(5)
		let startWidth = width / 3
		let startHeight = height / 8
		rect(width / 2, height - height / 3, startWidth, startHeight)
		fill('white')
		textSize(30)
		textAlign(CENTER)
		text("START", width / 2, height - height / 3 + 10)
		fill('rgb(241,163,241)')
		let titleWidth = width / 2.5
		let titleHeight = height / 3
		rect(width / 2, height / 2 - height / 4, titleWidth, titleHeight)
		fill('rgb(73,73,73)')
		stroke('white')
		textSize(titleWidth / 10)
		text("BINARY GAME", width / 2, height / 2 - height / 3.5)
		text("RECREATION", width / 2, height / 2 - height / 5)
	}
}

function Question() {
	let questiondifflevel = 3
	if (answer == 0) {
		answer = (Math.round(random(min, max)))
	}

	if (sum == answer) {
		Score += answer
		difficulty = 0
		min = 0
		max = 0
		answer = (Math.round(random(min, max)))
	}

	if (difficulty <= 0) {
		difficulty = (Math.round(random(questiondifflevel)))
	}

	if (difficulty == 1 && min == 0 && max == 0) {
		min = 1
		max = 32
	}

	if (difficulty == 2 && min == 0 && max == 0) {
		min = 32
		max = 128
	}

	if (difficulty == 3 && min == 0 && max == 0) {
		min = 64
		max = 511
	}

	textSize(30)
	fill('rgb(234,74,74)')
	text('Number to get: ' + answer, width - 200, 100)
	let difficultycolor = 'rgb(255,255,255)'
	if (difficulty == 1) {
		difficultycolor = 'rgb(22,251,0)'
	}
	if (difficulty == 2) {
		difficultycolor = 'rgb(229,255,0)'
	}
	if (difficulty == 3) {
		difficultycolor = 'rgb(255,0,0)'
	}
	fill(difficultycolor)
	textSize(width / 40)
	text('Difficulty Level: ' + difficulty, width - 200, 150)
	text('minimum: ' + min, width - 200, 200)
	text('maximum: ' + max, width - 200, 250)
}

function gameDone(){
	if (countdown == 0){
		clear()
		gameStart = 0
		textSize(width / 10)
		fill('white')
		stroke('black')
		strokeWeight(10)
		text('GAME FINISHED', width/2, height / 4)
		textSize(width / 25)
		strokeWeight(5)
		text('FINAL SCORE: ' + Score, width / 2, height / 2)
		textSize(width / 40)
		strokeWeight(3)
		text('score does not save sorry', width / 2, height / 2 + height / 6)
	}
}
