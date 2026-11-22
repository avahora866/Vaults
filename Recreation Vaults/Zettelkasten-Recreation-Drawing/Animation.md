# The Mechanics of Time
## Frame
In the physical world, time flows continuously. In animation, time is discreet. It is broken down into individual slices called **Frames**. A frame is simply a single still image.
## Frame Rate (FPS)
This is the heartbeat of your animation. **FPS** stands for **Frames Per Second**. It dictates how many of those still images are displayed within one single second of time.
- **The Standard:** The cinematic standard for most 2D animation is **24 FPS**.
- **The Math:** This means that for every **1 second** of animation you watch, the computer (or projector) is flashing **24** distinct "slots" of time before your eyes.

#### The Illusion (Persistence of Vision)

Why do we do this? Because of a phenomenon called _persistence of vision_. If we show images fast enough, the human brain ignores the gaps between them and perceives continuous motion.

> **The Flipbook Analogy:** Imagine a thick notepad. On each page, you draw a ball slightly further to the right than the previous page. Each page is a **Frame**. The speed at which you flip the pages with your thumb is the **Frame Rate**. If you flip too slowly (low FPS), you just see separate drawings. If you flip quickly (high FPS), the ball appears to move across the page.

# The Animator's Interface
To animate, you do not draw frame 1, then frame 2, then frame 3 linearly. That is a recipe for shrinking characters and jittery motion. Instead, we use a structured workflow assisted by specific tools.

## Keyframes vs. In-betweens
We divide drawings into two categories:

- **Keyframes (Keys):** These are the most important drawings. They tell the story. If a character punches a wall, the **Keys** are:
    1. The character winding up the fist.
    2. The moment of contact with the wall.
    3. The aftermath (pain).
- **In-betweens:** These are the frames that go _between_ the Keyframes. They do not add new information; they simply smooth out the transition from Key A to Key B.

## Onion Skinning
You asked about the meaning behind "Onion Skin." This term comes from the traditional era of hand-drawn animation.

**The History:** Before computers, animators drew on paper. To see the previous drawing while drawing the current one, they used very thin, semi-transparent paper that resembled the **skin of an onion**. They would stack these sheets on a lightbox.

**The Digital Tool:** In software like Krita, **Onion Skinning** is a feature that mimics this transparency.
- When you toggle it on, the software makes the current frame opaque.
- It displays the **previous** frames in a faint color (usually **Red**).
- It displays the **future** frames in a faint color (usually **Green** or Blue).

This allows you to draw the "In-between" right in the middle of the Red line (where you came from) and the Green line (where you are going), ensuring smooth motion.

# The 12 Principles of Animation
In the 1930s, animators at Walt Disney Studios developed **"The 12 Principles of Animation."** These are not merely suggestions; they are the fundamental laws of physics for the cartoon universe.

The most important principles are: #TODO Other principles
## Squash and Stretch
This principle gives an object weight, flexibility, and life. Without it, your animation will look like rigid cardboard moving across the screen.
- **The Concept:** When an object moves fast or hits an immovable force, its shape changes, but its **volume** remains the same.

Let us return to our bouncing ball example.
1. **The Stretch (Speed):** As the ball falls and gains speed, it should not remain a perfect circle. It should elongate (stretch) vertically. This motion blur trick makes the fall look fast.
2. **The Squash (Impact):** When the ball hits the ground, the energy must go somewhere. The ball compresses (squashes) flat against the floor.
3. **The Recoil:** As it bounces up, it stretches again before returning to a normal circle at the peak of its bounce.
### The Golden Rule: Conservation of Volume
This is where beginners fail.
- If you squash the ball down (make it shorter), you **must** stretch it out to the sides (make it wider).
- If you stretch the ball (make it taller/thinner), you **must** make it narrower.

If you only flatten the ball without widening it, the ball looks like it is deflating or shrinking. If you stretch it without narrowing it, it looks like it is growing in size.

## Timing and Spacing
You might assume that if you want something to move across the screen, you just draw it moving in equal steps every frame.
- Frame 1: 0 cm
- Frame 2: 1 cm
- Frame 3: 2 cm

This is called **Linear Motion**, and it looks robotic and unnatural. Nothing in nature moves at a constant speed from start to stop. To create life, we must master **Timing** and **Spacing**.

### Timing
Timing is simply **how long** an action takes.
- If a ball moves from Left to Right in 24 frames, that is "Slow Timing" (1 second).
- If a ball moves from Left to Right in 6 frames, that is "Fast Timing" (0.25 seconds).

**Timing = The total number of frames.**
### Spacing
Spacing is **where** the drawings are placed on those frames. This controls the _acceleration_ and _deceleration_.

**Slow In and Slow Out (Ease In / Ease Out)** In the real world, things need time to speed up and slow down. A car does not go from 0 to 60 instantly.
- **Slow Out:** When an object starts moving, the drawings are close together (spaced tightly). It is slowly "moving out" of its pose.
- **Fast Action:** In the middle of the movement, the drawings are far apart.
- **Slow In:** When the object stops, the drawings get close together again. It is "slowing into" its final pose.

> **The Pendulum Analogy:** Think of a grandfather clock pendulum.
> 
> - At the very top of the swing (left and right), it hangs in the air for a split second. (Drawings are close together).
> - At the bottom, it swings through very fast. (Drawings are far apart).

## Arcs
In nature, almost nothing moves in a perfectly straight line.
- **Mechanical objects** (pistons, elevators, sliding doors) move in straight lines.
- **Organic objects** (humans, animals, plants) move in **Arcs**.
	
If you animate a hand moving from Point A to Point B in a straight line, the character will look like a robot. If you animate it moving in a curved path (an Arc), it will look alive.

### The Physics: Why Arcs Happen
Why does this happen? It goes back to your "Stick Man" armature.
Your body is a system of levers connected by **Pivot Points** (joints).
- Your arm is not a piston; it rotates around the **Shoulder**.
- Your forearm rotates around the **Elbow**.
- Your hand rotates around the **Wrist**.

Because these limbs rotate around a central point, the path they trace is circular. A movement is simply a slice of that circle—an **Arc**.

### The "Head Turn" Example
This is the most common mistake beginners make.
Imagine a character turning their head from looking **Left** to looking **Right**.
- **The Beginner Approach:** Draws the nose travelling in a straight horizontal line across the face.
- **The Result:** It feels like the head is sliding on a rail. It feels weightless.
- **The Professional Approach:** The nose should dip down slightly in the middle of the turn (an arc).
- **The Result:** This implies the weight of the head and the mechanics of the neck muscles.

#### Correcting Your In-Betweens
When you are drawing your in-betweens (the frames between the Keyframes), you must be vigilant.

If you have a Keyframe of a hand at the waist, and a Keyframe of a hand high in the air, your brain will want to draw the in-between exactly in the middle of a straight line connecting them. You must consciously fight this urge and place the in-between **offset** to create the curve.

## Anticipation
There is a rule in theater and animation: **"Tell them what you are going to do. Do it. Then tell them that you did it."**

**Anticipation** is the first part: "Tell them what you are going to do."

### The Physical Necessity (The Wind-Up)
In the real world, you cannot generate force from nothing.
- To jump **Up**, you must first bend your knees **Down**.
- To throw a ball **Forward**, you must first pull your arm **Back**.
- To sprint **Right**, you must shift your weight **Left**.

This "opposite movement" builds the potential energy required for the main action. Without anticipation, movements look sudden, weak, and magical (like a video game character glitching).

### The Communication Necessity (Eye Guidance)
Animation moves fast. If a character suddenly zips off-screen without warning, the audience might miss it.

Anticipation acts as a signal. It says to the audience: _"Look here! Something is about to happen!"_ By pulling back for just 6-8 frames (a fraction of a second), you give the viewer's eye enough time to focus on the character, ensuring they see the punch or the jump when it happens.
### The Formula
A standard action consists of three parts:
1. **Anticipation (The Prep):** The move in the opposite direction.
2. **Action (The Move):** The actual activity (punch, jump, run).
3. **Follow-Through (The Recovery):** The settling of energy after the move (we will cover this later, but know it exists).

### Subtle Anticipation
Not all anticipation is big physical movement. It can be small.
- Before a character walks to the door, they might shift their **eyes** to look at the door first.
- Before a character speaks, they might take a small **breath** (chest expands).

Even these tiny movements make the character feel like a thinking, breathing creature rather than a puppet.

# The Workflow (From Structure to Skin)
## The "Stick Man" Fallacy
Novices often draw a stick man and try to animate it as the final product. This limits you because a stick figure has no **volume**. You cannot rotate a stick figure easily because a line looks the same from the front as it does from the side.

Instead, we use the "stick man" only as a structural guide—a skeleton upon which we hang the meat of the character.

## The Standard Workflow
To animate a scene (for example, a character waving), you should follow these distinct stages. Do not try to do them all at once.

**Phase 1: The Armature (The Stick Man)**
- Draw a simple skeleton. A circle for the head, a line for the spine, lines for arms.
- **Focus:** Is the pose clear? Is the timing right?
- _Do not worry about details like fingers, eyes, or clothes._

**Phase 2: The Rough (The Mannequin)**
- Add volume to your stick man.
- Turn the line of the arm into a cylinder. Turn the circle of the head into a sphere.
- **Focus:** Does the character feel 3D? Does the squash and stretch work?

**Phase 3: Tie-Down / Clean-Up**
- This is where you draw the final "pretty" lines, clothing details, and facial features on top of your rough animation.
- **Focus:** Line quality and consistency.

By working this way, you solve the movement problems _before_ you waste time drawing buttons on a shirt.