Degree level: Master’s

# Project proposal

## 1. The problem and goal

(Explain the problem you want to solve and why it matters. Describe what you
want to improve, how you will measure improvement, and which limits or
requirements must be respected. A short example can help someone outside your
field understand the problem.)

**The problem and goal in my project:**

Designing a small fixed-wing drone is a slow loop: build a shape (body, wing,
tail, steering flaps), simulate the airflow around it, read the results, decide
what to change, and repeat. My goal is an AI agent that runs this loop: given requirements, it proposes a design, reads
the simulation results and improves the design until the requirements are met,
or explains why they cannot be.

What improves: the lift-to-drag ratio (L/D) at cruise, including an estimate of profile drag, the lift produced divided by the air resistance. A higher value means less energy is needed to stay airborne. The agent is judged on how often it meets all requirements across several test cases, how many simulation runs it needs, and how it compares with a baseline search program given the same run budget.

Limits (initial values, to be confirmed): 10 kg total mass including a 5 kg payload, wingspan up to 2.5 m, cruise at 60 m/s at sea level, mean wing chord of at least 0.12 m, and a static margin between 5% and 15% of mean chord. The aircraft must be trimmed (forces and turning moments balanced for steady flight) and stable (it returns to its attitude after a disturbance).

Traceability: 
every reported number comes from a logged simulation run.

## 2. The simulator

(Describe a program or model that could simulate your system and calculate what
happens under different conditions. Explain what inputs it takes and what
results it produces. State whether it can run on your computer and accept inputs
in a text file, such as JSON or YAML. If you have not chosen a simulator yet,
name an option you want to explore and explain what you need to check.)

**The simulator in my project:**

I plan to use OpenVSP, a free, open-source program that builds aircraft shapes
from adjustable parameters, with its built-in solver VSPAERO. The solver uses a
simplified airflow model (a vortex-lattice method) that runs in seconds (a full 3D fluid simulation
takes hours).

Inputs:
shape parameters (wing, tail, body and flap dimensions) and flight
conditions (speed, altitude, angle to the airflow).

Outputs:
lift, drag and turning-moment coefficients, L/D, static margin,the angle needed for trim, and a solver convergence flag.

Runs locally:
yes, through a Python interface. The agent writes its choices
as a JSON file, and my tested code builds the model and runs the solver.

To check:
that the interface installs and a textbook wing case matches its
known answer. The solver does not model stall, so I make no stall claims.

## 3. The agent's role

(Explain what the language-model-based agent will decide or change, and how it
will use the simulator's results to choose its next action. If known, list the
variables it may change and their allowed ranges. Explain what you expect the
language model to contribute: could a simple program that tries different
options and picks the best result achieve the same goal? Include one question
you still need to answer.)

**The agent in my project:**

The agent acts as the design engineer: it picks a starting design, calls tools
to build and simulate it, reads the numbers and decides what to change. It stops
when the requirements are met, its run budget is used up, or it concludes they
cannot all be met.
Variables it may change:(ranges to be fixed in the specification): tail
arrangement, wing length, width, taper and sweep, tail size and position, flap
size, body size.
What the language model adds:
a simple search program can tune numbers, and
I will build one as a baseline. The agent should add value in choosing between
very different layouts, diagnosing failed runs, and balancing conflicting requirements.

Open question: for which requirement sets does the agent do better than the baseline, and how many repeated runs are needed to show that reliably?

## Explain your project to someone outside your field

(Now describe your whole project again, as if you were explaining it to someone
from another field who has no background in your topic. For example, if you
study chemical engineering, imagine explaining it to a physicist; if you study
aeronautical engineering, imagine explaining it to someone in medicine; if you
study data science, imagine explaining it to a materials engineer. In one or two
short paragraphs, connect all three parts: the problem you want to solve, how
the simulator helps you test ideas, and what the agent will decide or change.
Use everyday language, explain any technical terms you need, and avoid
unexplained abbreviations. Do not simply copy your answers above.)

**My project in simple words:**

Designing an aircraft means choosing its shape so that it flies efficiently and
steadily. Engineers simulate the air flowing around a digital model and adjust the shape
based on the numbers, over and over. The simulator cannot decide what to change.

My project gives that decision to an AI assistant. Told the requirements, such
as the load and maximum size, it builds a digital aircraft, asks the simulator
how it would fly and changes the shape until the requirements are met. To check
that the AI is useful, I will compare it with a trial-and-error program.


