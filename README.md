# Culminating-task-Gr.10
Computer Science culminating task.

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
	Bounds = Matter.Bounds,
	Body = Matter.Body,
	Bodies = Matter.Bodies;

let engine, render

function setup() {
	MyWorld()

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
			background: 'rgb(136,136,136)',
		}
	});

	// This lets use draw on the canvas using processing's (non-physics) drawing tools
	createCanvas(windowWidth, windowHeight, render.canvas)


	// Creates a mouse constraint
	var mouse = Mouse.create(render.canvas),
		mouseConstraint = MouseConstraint.create(engine, {
			mouse: mouse,
			constraint: {
				stiffness: 0.2,
				render: {
					visible: true
				}
			}
		});

	// Adds the mouse constraint
	Composite.add(engine.world, mouseConstraint)


	// Creates the walls
	TopWall = Bodies.rectangle(0, 0, width * 2, 100, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});
	BottomWall = Bodies.rectangle(0, height, width * 2, 100, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});
	LeftWall = Bodies.rectangle(0, 0, 100, height * 2, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});
	RightWall = Bodies.rectangle(width, 0, 100, height * 2, {
		isStatic: true,
		render: {
			fillStyle: '#A7A7A7'
		}
	});

	// Creates shapes
	Ball = Bodies.circle(width / 2, height / 2, 20, {
		restitution: 1,
		render: {}
	})

	var Lstack = Composites.stack(150, 200, 1, 5, 0, 0, function(x, y) {
		return Bodies.circle(x, y, 20, {
			isStatic: false,
			restitution: 1
		});
	});

	// Adds all the bodies
	Composite.add(engine.world, [TopWall, BottomWall, RightWall, LeftWall, Ball, Lstack])

	// run the renderer
	Render.run(render);

	// create runner
	var runner = Runner.create();

	// run the engine
	Runner.run(runner, engine);
}
