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

[behavior trees](https://arxiv.org/abs/1709.00084)

[object process methodology](https://en.wikipedia.org/wiki/Object_Process_Methodology)

[robot operating system version 2](https://index.ros.org/doc/ros2/)

## Past

Ten years ago or so, I was working in the Dickinson Lab at Caltech along with
about half the people who work at Janelia now. Michael Dickinson would observe
plates of flies and notice patterns in their behavior. This is a nice easy way
of studying flies, but it can take a long time. If there was a particular
interaction between two flies that he wanted to see, he would need to watch and
wait for it to occur naturally.

In order to increase the number of observations of the two-fly behavior, he
thought of putting a little magnet on top of the plate, next to the flies so
they could see it. Then he could take another magnet underneath the plate and
use it to move the top magnet around like one of the two flies in the behavior.
A fly might see that magnet and mistake it for another fly and perform the
two-fly behavior he wanted to see.

A science system may be as simple as just one smart scientist theorizing about
and experimenting with the world using basic instruments, converting
observations into knowledge.

There can be advantages, though, to making that very simple science system more
complex. Adding automation, for example, can help collect larger quantities of
data in a shorter amount of time and in a more repeatable fashion, freeing
scientists to use their brains and time for more interesting and challenging
aspects of the research.

Kristin and Alice and others did a brilliant job of automating the observation
of fly behaviors using cameras, image processing, and machine learning. A camera
above the plate of flies can watch them for hours, calculating their positions
and orientations over time and automatically categorizing the behaviors that it
recognizes. Automation never gets tired or bored, so it can vastly increase the
amount of data collected and the number of researchers capable of collecting it.

To increase the number of observations of a two-fly behavior even further,
Michael then asked me to also automate the magnet interaction trick, using
motors, controllers, and software to replace the manual motions of the
researcher hand on the bottom magnet. Automating both the sensing and actuation
allows a feedback loop around the little fake magnet fly to mimic extremely
simplified behaviors of a real fly.

I found it extremely fascinating to combine automation and feedback loops to
study animal behaviors and I have been working on similar experimental rigs and
projects ever since.

The basic problem of automating magnet interactions with flies is pretty
straightforward. You observe the fly and the magnet with a camera, process the
camera images, and then send motion commands to the motor controllers to moved
the magnets. We wanted to take images of the flies at 100 Hz, so that gave 10 ms
to do all of the processing and communication before the next image arrived.

At first I mistakenly thought that this is a very easy problem. I assumed that
if the observations and control was accurate enough and if the processing and
communication was fast enough to achieve acceptable performance, it was just a
simple matter of connecting all the components of the system together properly.

Really, though, I found the bigger challenge to be doing everything else a
researcher might reasonably expect from a scientific instrument, along with and
at the same time as all of that processing and control. Saving the data,
interacting with the user, describing desired behaviors of a fake fly with
respect to a real fly, being easy enough to understand to operate and modify by
someone who is not necessarily an experienced programmer, are all challenges on
top of the basic control problem that make it much more difficult.

The base software code to close the feedback loop was small and simple, but
adding feature after feature, multi-threading to mimic concurrency, I was
struggling to keep the software from becoming a bloated complicated mess with
poor performance. I was complaining about this to Martin and he told me about
some software his friend was working on, called the Robot Operating System,
which originated from Stanford.

The Robot Operating System (ROS) is a framework for splitting software into
relatively small distributed independent nodes which send messages to each
other. These nodes may all run on the same computer or on multiple computers
connected in a network. It was designed to control robots, as the name suggests.

I have found that makes some people, both scientists and roboticists, feel as if
it is inappropriate to use something designed for robots on scientific
instruments. Many people have a somewhat narrow view of the forms a robot can
take, thinking all robots must either look humanoid, or like an industrial robot
arm, or a drone. Many scientific instruments, though, and especially many
behavioral rigs, have sensors, actutators, computers, and controllers, just like
any other robot, except in a rearranged form.

The first tutorial on the ROS website involves driving a simulated turtle around
in a two dimensional window, drawing lines wherever it goes. This is an homage
to the old LOGO educational programming language. I was intrigued from the start
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

Using a set of small, easy-to-understand, nodes and combining them together is
akin to the Unix philosophy of making each program do one thing well and
combining small programs to make a more complex program, rather than adding
feature after feature to a single large program until it is too complicated to
understand.

I think in the end the software using the ROS framework ended up working well.
There are many ways which the software could have been written an any way that
worked would have been acceptable. Splitting the software into small independent
nodes made intuitive sense to me at least, making it far easier for me to work
on it and understand it. I was never able to figure out how to make it easy for
a relatively inexperienced programmer to use it however and I have been trying
to figure that out ever since.

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
lights and sounds. These rigs are still being used today, years after being
built.

I enjoyed using the Robot Operating System and thought it was a great way to
control experiments. Nodes were written in either C++ or Python running on Linux
computers, which I liked, but many scientists, who only want to use Windows and
Matlab, did not. Eventually I decided that the lack of support for Windows and
Matlab was a deal breaker for too many scientists, so I stopped using ROS.

I liked the way ROS made it easy to connect nodes written in various languages.
I just wished it worked with Windows and Matlab, so I decided to create my own
simplified version. I spent the next few years developing a method for sending
JSON strings to and from software nodes written in either Python, C++, or
Matlab, and to and from custom circuit board nodes running firmware written in
C++. It did not perform as well as ROS, but since it was my own custom creation
I could make work just as I wanted.

I created custom circuit boards with microcontrollers running code that I wrote
for controlling motors, and valves, and speakers, and hobby servos, and all
sorts of other actuators. I also had boards that interfaced to a variety of
analog and digital sensors. Each board was small and simple, doing just one
thing well, and more complex controllers could be constructed by connecting as
many small boards together as necessary. Boards sent messages to each other much
like ROS sends messages between nodes. It used a client/server model, which is
not nearly as nice or flexible as the ROS publish/subscribe model, but that made
it easier to implement.

I got this system to work well and used it on many projects and rigs for a
variety of labs over the years. Eventually, though, I realized that being a
totally custom solution was a fatal flaw of this approach. I had used some
conventional standards whenever I could, like JSON and JSON-RPC, but many of the
interfaces I had to just make up on my own. I realized I was never going to be
able to convince the world to adopt all of the interfaces I had created, so my
custom solution was never going to grow beyond what I could personally make an
maintain.

So I abandoned my custom approach, just as I abandoned ROS earlier, and spent
time trying to figure out what to use or create next. I wanted something that
would work well for the projects and experimental rigs I was building, but also
be usable and supported by a large community of people out in the world for a
variety of other problems.

In order to interoperate with the rest of the world, it is important to use what
the rest of the world has already accepted. The issue, of course, is that there
are many possible ways of solving such problems and everyone has his or her own
opinion and preferences. It is impossible to get everyone to agree on a single
approach.

So systems are created with a collection incompatible components. Most of the
system development time is spent gluing together these inconsistent pieces,
forcing them to fit together.

While getting everyone to use the same approach is impossible and agreeing on
anything is extremely challenging, spending time to find and adopt some
interoperable standards that would allow all of these different approaches to
work together more easily would have huge payoff.

## Present

I am working on several projects now where I have to decide on how to construct
the system architecture from very disparate pieces. One of those projects is the
SmartCage. Multiple sensors and actuators inside a cage need to be controlled in
order to train and experiment with mice. Multiple SmartCages will be used
simultaneously, so cages will need to be connected in a network to be monitored,
controlled, and updated remotely. Every lab and every researcher has a different
preference of software and hardware to use on and with the SmartCages. So it is
an interesting challenge to figure out how to design the system architecture to
work with all of these preferences.

There are many possible ways to create such a system architecture. After doing
lots of research, there are three topics that I want to explore further to make
such architectures interoperable and easy to use, the new version of ROS (ROS
2), behavior trees, and some system modeling language, such as the Object
Process Methodology (OPM).

### ROS 2

The people who were working on ROS realized that it had some flaws and they made
some huge changes and improvements to it and released it as ROS 2. ROS 2 has all
of the advantages of ROS 1, but now runs on Windows and Mac as well as Linux and
now can be used with Matlab and Java and variety of other languages as well as
Python and C++. It uses an industry standard called the Data Distribution
Service (DDS) to communicate between nodes. It was designed to connect multiple
robots as well as control a single individual.

I think ROS 2 would work really well for projects like the SmartCage.
Researchers running Matlab nodes could easily communicate with hardware devices
and software nodes written in other languages.

[ROS 2](https://index.ros.org/doc/ros2/)

### Behavior Trees

One issue that still remains with a framework like ROS 2 is how to allow
researchers, who may not know how to program in any software language, to modify
the behavior of their experimental rigs without requiring someone else to
reprogram it for them.

The video game industry has this problem too. Video games are written in
languages like C++ and C#, but people who do not program in those languages
still want to be able to design and modify virtual game characters and play.

So people in the video game industry invented a technique named behavior trees
to solve to solve this issue. Behavior trees are a way of structuring behavior,
or task switching, similar to state machines, but in a way that is more modular
and arguably easier to use.

Behavior trees can be created and modified in graphical programs or text files.
It is a programming language in its own right, but a domain specific language
designed to be easy to learn and understand for the specific problem of
organizing a set of behaviors. More general programming languages, such as
Python or Matlab, are more time consuming to learn since they were designed to
work on more types of problems.

Each component in a behavior tree is some action, for example "steal stuff" or
"fight cops" in some violent video game. Actions may take time to complete and
always end in either success or failure. These actions may be arranged to take
place in a sequence, occurring one after the other as long as the previous
action succeeds. Or they may be arranged so that some action occurs only if some
previous action fails. These simple rules allow simple behaviors to be combined
into complex behaviors that are still easy to visualize and understand.

Actions may be composed of behavior trees themselves or they may be written in
some more general language such as C++ or Python. Actions can be added or
removed or rearranged without affecting the inner workings of other actions,
making them very modular and easy to reuse and change.

State machines are another, extremely common, way of performing task switching.
Unlike behavior trees, adding or removing states in a state machine often
requires modifiying other states, making them less modular and more difficult to
reuse and change.

State machines might be compared to a GOTO statement in software, where the
execution of a program jumps to another part of the code and continues executing
from there. This is called a one-way control transfer and was used often in
early programming languages, but is avoided in modern programming languages
because of the difficulty in modifying and understanding GOTO code.

Behavior trees might be compared to software function calls that have two-way
control transfer. Here the execution jumps to a particular part of the code,
executes it, then returns to where the function call was made. This makes it
easier to remove the function call if necessary without worrying about where the
exectution needs to jump to next.

People in robotics are starting to see the advantages of using behavior trees to
control real hardware as well as virtual agents.

[behavior trees](https://arxiv.org/abs/1709.00084)

### Object Process Methodology

Yet another issue with heterogeneous science systems is that people from a
variety of fields may work on the same system, each using their own domain
specific language or model for their piece of the system. It can be very
difficult to keep all of these models and documents written in various languages
consistent and up-to-date. These inconsistencies can cause the system to fail,
adding time and cost to projects in order to fix, and make it very difficult for
projects to be replicated, especially by outside groups and researchers.

For example, a mechanical engineer might create a geometric model of hardware in
a mechanical CAD program to figure out where an electrical switch needs to be
placed. An electrical engineer may design a circuit board in an electrical CAD
program to connect that switch to a microcontroller input pin. A firmware
engineer may use that switch input as trigger to run some code that then talks
to some GUI software written by some other software engineer. The researcher may
depend on that GUI notification for some part of their experiment protocol. The
electrical switch may be listed in a bill of materials that a lab administrator
uses to order parts for duplicate rigs. That switch may be connected to the
circuit board by a cable made by an electrical technician with hand drawn
documentation of the pinout. Modifying something as simple as that switch may
affect all of those people and documents making it extremely time consuming to
track down all consequences of even small decisions.

These types of systems are document-based. The person in each field creates his
or her own documents and the collection of documents specifies the system. These
documents all depend on each other, but these dependencies may need to be
mantained manually, which is very time consuming and error-prone. This is
similar to old architectural drawings. Many 2D drawings were used to specify a
3D structure like a building. These drawings depended on each other, but
modifying one did not automatically update all of the others, this needed to be
done manually by the architect.

One possible solution to this issue is to create a model of the entire system
and use it as the one source of truth. Documents might be automatically
generated from this model and regenerated when there are changes. This is
similar to modern architecture, where a 3D model of a structure is designed
first, then the 2D drawings are simply views of that model generated
automatically. Changes in the 3D model may be automatically updated in the 2D
drawings.

In order to create a model of an entire system, a language is needed to express
all of the components in that system and the connections between them.

Object Process Methodology (OPM) is a language created for modeling systems in
any domain based on a minimal universal ontology. In this ontology, things are
either objects or processes, with links connecting them. People have used OPM to
model software systems, robotic systems, biological cells, and human societies.
OPM is now an ISO standard, intended to be able to specify other ISO standards
in some language more precise and easier to understand than general English.

There are other modeling languages, such as UML or SysML, which could also be
used.

There is a danger in introducing yet another language into a system and
expecting others to learn it, though. Models can become rabbit holes where you
spend more time messing with the model than the real system or they can be
ignored and become yet another inconsistent document in the system making it
worthless or misleading.

Much of the same might be true for a mechanical CAD model, however, and yet
mechanical engineers almost always find it advantageous to create a mechanical
model of part or an assembly first before building it or generating 2D drawings
of it.

Perhaps if a system model was easy enough to create and understand it could be
advantageous to have one for every science system. At the very least perhaps it
could be used as a way to document a system after it has been completed.

[object process methodology](https://en.wikipedia.org/wiki/Object_Process_Methodology)

## Future

If we could all agree on some interoperable standards, creating, using, and
explaining science systems may be easier than it is now.

It could make sense to first create models of a science system, and of the
knowledge it is intended to discover, before building it. Those models might be
used as a guide for the creation and use of the system and as a way to document
and explain the system afterwards.

ROS 2 could be a way to connect heterogenous hardware and software into a larger
system. Each hardware and software component could be a node running in a Docker
container. Using containers could make it easier to update nodes and ensure
consistent performance in a variety of environments.

If both custom hardware and off-the-shelf hardware had identical node
interfaces, those components could be used interchangeably. Nodes could then be
reused in multiple projects so they do not need to be developed from scratch
every time.

If each node was capable of performing a set of actions, then behavior trees
could be used to organize those nodes to perform complex behaviors. Those
behaviors might be modified by someone without requiring knowledge of a general
programming language.

Behavior trees might be used for more than control. Perhaps software that
automatically observes and classifies behavior could generate or rearrange
behavior trees to help explain animal or other observed behaviors in an
experiment.

Behavior trees seem similar to Object Process Methodology. Perhaps those
concepts could be combined somehow. Perhaps behavior trees are subsets of Object
Process diagrams where only processes are used. Perhaps OPM can be expanded to
make geometric layout of the diagram meaningful, just as the geometric layout of
behavior trees has meaning with respect to the order that the processes are run.
Perhaps behavior trees can be expanded to include the concept of objects as well
as processes.

Maybe in the end it will just be too difficult to get anyone to agree on
anything, but if there are other ideas of how to create and organize science
systems I would love to hear them.

Thank you for your time!
