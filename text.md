---
layout: default
---

In this time of weirdness, I will not attempt to make a formal presentation.
Instead I will write out some unpolished thoughts that I wanted to present and
hopefully my internet will work well enough to have an informal discussion about
them over video chat at the allotted meeting time. If not, then anyone can feel
free to email me any thoughts at any time: polidorp@janelia.hhmi.org

# Science Systems

Modern science is often a very interdisciplinary endeavor. People from a variety
of fields and backgrounds must work together to develop new theories and models,
design and perform experiments, train animals, build scientific instruments to
collect data, analyze the data to discover knowledge, and disseminate and teach
that knowledge to the rest of the world. Science is a diverse system and each
field has its own domain specific language with its own vocabulary and
conventions. Those languages have a great deal of overlap, but connecting this
heterogeneous collection of people and parts is often a real challenge.

I have been looking for ways to help make the people and other components of
science systems more interoperable. I have been finding that getting people to
agree on anything is a way more difficult challenge than any technical problem.
There are three topics I have been thinking about lately that perhaps could help
connect scientific components in a system and I would like to discuss them with
people and see if anyone agrees before using them in future projects:

[robot operating system version 2](https://index.ros.org/doc/ros2/)

[behavior trees](https://arxiv.org/abs/1709.00084)

[object process methodology](https://en.wikipedia.org/wiki/Object_Process_Methodology)

## Past

Ten years ago or so, I was working in the Dickinson Lab at Caltech along with
about half the people who work at Janelia now. Michael Dickinson would observe
plates of flies and notice patterns in their behavior. He thought of putting a
little magnet on top of the plate next to the flies so they could see it and a
magnet underneath the plate so he could move it with his hand and interact with
the flies and influence their behavior. A science system may be as simple as one
smart scientist theorizing about and experimenting with the world using basic
instruments, converting observations into knowledge. There can be advantages,
though, to making that science system more complex. Adding automation, for
example, can help collect larger quantities of data in a shorter amount of time
and in a more repeatable fashion, freeing scientists to use their brains and
time for more interesting and challenging aspects of the research.

Kristin and Alice and others did a brilliant job of automating the observation
of fly behaviors using cameras, image processing, and machine learning. This
automation vastly increased the amount of data collected and the number of
researchers capable of collecting it.

Michael then asked me to automate the interaction aspect of the experiments
using motors, controllers, and software to replace the manual motions of the
researchers' hands on the magnet. I mistakenly thought this was an easy problem
at first, but I found it fascinating enough that I have been working on similar
experimental rigs and projects ever since.

The basic problem of automating magnet interactions with flies is pretty
straightforward. You observe the fly and the magnet with a camera, process the
camera images, and then send motion commands to the motor controllers. I assumed
the main issue was just making that happen quickly and accurately enough to have
acceptable performance. Really, though, I found the bigger challenge to be doing
all of that along with everything else a researcher might reasonably expect from
a scientific instrument, like saving the data, interacting with the user, being
easy enough to understand to operate and modify by someone who is not
necessarily an experienced programmer. I was struggling to keep the software
from becoming a bloated complicated mess with poor performance when Martin told
me about some software his friend was working on, called the Robot Operating
System, which originated from Stanford.

The Robot Operating System (ROS) is a framework for splitting software into
relatively small distributed nodes which send messages to each other. It was
designed to control robots, as the name suggests, and I have found that makes
some people, both scientists and roboticists, feel as if it is inappropriate for
scientific instruments. Many people have a somewhat narrow view of robot forms,
thinking all robots must either look humanoid, like an industrial robot arm, or
a drone. Many scientific instruments, though, and especially many behavioral
rigs, have sensors, actutators, computers, and controllers, just like any robot.

The first tutorial on the ROS website involves driving a simulated turtle around
in a two dimensional window, drawing lines wherever it goes. This is an homage
to the LOGO educational programming language. I was intrigued from the start
since this was exact problem I was trying to solve, driving a magnet around on a
two dimensional plate.

I rewrote all of my magnet automation software to fit into the ROS framework and
made a working experimental rig capable of interacting with flies and collecting
data. I split the problem into a set of nodes running independently, sending
each other messages. For example, there was a node which acquired images from
the camera and published them to any node who may want to subscribe, like an
image saving node, an image display GUI node, and image processing nodes. There
was a node that interacted with the motor controllers and could receive messages
from a keyboard node, or a joystick node, or an automatic control node.

I think in the end the software ended up working well. There are many ways which
the software could have been written an any way that worked would have been
acceptable. Splitting the software into small independent nodes made intuitive
sense to me at least, making it far easier for me to work on it and understand
it. I was never able to figure out how to make it easy for a relatively
inexperienced programmer to use it however and I have been trying to figure that
out ever since.

It would have been fascinating to combine the interaction software with Kristin
and Alice's automatic behavioral analysis software so that the motion of the
magnet could have changed based on the behaviors of the flies in real-time. Back
then the software and hardware was not fast enough to make that possible, but
now that some of you have made that work in mere milliseconds, all sorts of new
and exciting experiments may be possible.

My first few years at Janelia I built several rigs using the Robot Operating
System. For Ulrike's lab I made a fly alcohol assay that detected flies with a
camera and actuated gates and valves to expose flies to alcohol and observe
their behaviors. For Marta's lab I made a larva tracker that moved a camera to
keep a larva in the field of view and expose it to a variety of stimuli, like
lights and sounds.

I enjoyed using the Robot Operating System and thought it was a great way to
control experiments. Nodes were usually written in either C++ or Python running
on Linux computers, which I liked, but since many scientists who only want to
use Windows and Matlab did not. Eventually I decided that the lack of support
for Windows and Matlab was a deal breaker for too many scientists, so I stopped
using ROS.

I liked the way ROS made it easy to connect nodes written in various languages,
so I spent the next few years developing a method for sending JSON strings to
and from software written in either Python, C++, or Matlab, and to and from
custom circuit boards running firmware written in C++. I had circuit boards for
controlling motors, and valves, and speakers, and hobby servos, and all sorts of
other actuators. I also had boards that could receive inputs from a variety of
analog and digital sensors. Each board was small and simple and complex
controllers could be constructed by connecting as many small boards together as
necessary. Boards sent messages to each other much like ROS sends messages
between nodes. I got this system to work well and used it on many projects and
rigs over the years. Eventually, though, I realized that being a totally custom
solution was a fatal flaw in this approach. I had used some conventional
standards, like JSON and JSON-RPC, but I was never going to be able to convince
the world to accept all of the other standards that I had just made up on my
own.

In order to interoperate with the rest of the world, it is important to use what
the rest of the world has already accepted. The issue, of course, is that
everyone has his or her own opinion about what is acceptable, so systems are
created with incompatible components. Most of the system development time is
spent gluing together inconsistent pieces, so while looking for common ground is
extremely challenging, agreeing on some interoperable standards would have huge
payoff.

## Present

I am working on several projects now where I have to decide on how to construct
the system architecture. One of those projects is the SmartCage. Multiple
sensors and actuators inside a cage need to be controlled in order to train and
experiment with mice. Multiple SmartCages will be used simultaneously, so cages
will need to be connected in a network to be monitored, controlled, and updated
remotely.

After giving up on ROS and my own custom solution, I spent time researching
other possible approaches to these sorts of projects. There are many
possibilites, but the one that kept coming up over and over again in my searches
was the second version of the Robot Operating System.

### ROS 2

The people who were working on ROS realized that it had some flaws and they made
some huge changes and improvements to it and released it as ROS 2. ROS 2 has all
of the advantages of ROS 1, but now runs on Windows and Mac as well as Linux and
now can be used with Matlab and Java and variety of other languages as well as
Python and C++. It uses an industry standard called the Data Distribution
Service (DDS) to communicate between nodes.

I think ROS 2 would work really well for projects like the SmartCage.
Researchers running Matlab nodes could easily communicate with hardware devices
and software nodes written in other languages.

### Behavior Trees

One issue that still remains is how to allow researchers, who many not know how
to program in any software language, to modify the behavior of their
experimental rigs without requiring someone to reprogram it for them.

The video game industry has this problem too. Video games are written in
languages like C++ and C#, but people who do not program in those languages
still want to be able to design and modify virtual game characters and play.

So people in the video game industry invented a framework named behavior trees
to solve to solve this issue. Behavior trees are a way of structuring behavior,
or task switching, similar to state machines, but in a way that is more modular
and arguably easier to use.

Behavior trees can be created and modified in graphical programs or text files.
Each component in a behavior tree is some action, for example "steal stuff" or
"fight cops" in some violent video game. Actions may take any amount of time and
always end in either success or failure. These actions may be arranged to take
place in a sequence if some previous action succeeds or as a fallback when some
previous action fails. Actions may be composed of behavior trees themselves or
they may be written in some like C++ or Python. Actions can be added or removed
or rearranged without affecting the inner workings of other actions, making them
very modular and easy to reuse and change.

State machines are another, extremely common, way of performing task switching.
Unlike behavior trees, adding or removing states in a state machine often
requires modifiying other states, making them less modular and more difficult to
reuse and change. State machines might be compared to a GOTO statement in
software, where the execution of a program jumps to another part of the code and
continues executing from there. This is called a one-way control transfer and
was used often in early programming languages, but is avoided in modern
programming languages because of the difficulty in modifying GOTO code.

Behavior trees might be compared to software function calls that have two-way
control transfer. Here the execution jumps to a particular part of the code,
executes it, then returns to where the function call was made. This makes it
easier to remove the function call if necessary without worrying about where the
exectution needs to jump to next.

People in robotics are starting to see the advantages of using behavior trees to
control real hardware as well as virtual agents.

### Object Process Methodology

Another issue with heterogeneous science systems is that people from a variety
of fields may work on the same system, each using their own domain specific
language or model for their piece of the system. It can be very difficult to
keep all of these models and documents written in various languages consistent
and up-to-date. These inconsistencies can cause the system to fail, adding time
and cost to projects in order to fix, and make it very difficult for projects to
be replicated, especially by outside groups and researchers.

For example, a mechanical engineer might create a geometric model of hardware in
a mechanical CAD program to figure out where a switch needs to be placed. An
electrical engineer may design a circuit board in an electrical CAD program to
connect that switch to a microprocessor input. A firmware engineer may use that
switch input as trigger to run some code that then talks to some GUI software
written by some other software engineer. The researcher may depend on that GUI
notification for some part of their experiment protocol. The electrical switch
may be listed in a bill of materials that a lab administrator uses to order
parts for duplicate rigs. That switch may be connected to the circuit board by a
cable made by an electrical technician with hand drawn documentation of the
pinout. Modifying something as simple as that switch may affect all of those
people and documents making it extremely time consuming to track down all
consequences of even small decisions.

One possible solution to this issue is to create a model of the entire system
and use it as the one source of truth. Documents might be automatically
generated from this model and regenerated when there are changes.

Object Process Methodology is a language created for modeling systems in any
field based on a minimal universal ontology. In this ontology, things are either
objects or processes, with links between them. People have used it to model
software systems, robotic systems, biological cells, and human societies.

There are other modeling languages, such as UML or SysML, which could also be
used.

There is a danger introducing yet another language into a system and expecting
others to learn it, though. Models can become rabbit holes where you spend more
time messing with the model than the real system or they can be ignored and
become yet another inconsistent document making it worthless or misleading.

Much of the same might be true for a mechanical CAD model, however, and yet
mechanical engineers almost always find it advantageous to create a mechanical
model of part or an assembly before building it.

Perhaps if a system model was easy enough to create and understand it could be
advantageous to have one for every science system. At the very least perhaps it
could be used as a way to document a system after it has been completed.

## Future
