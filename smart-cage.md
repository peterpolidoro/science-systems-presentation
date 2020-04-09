# SmartCage

## Description

The SmartCage system allows mice to perform voluntary head-fixations in their
home cages to facilitate behavior training and experiments without human
supervision.

## Overview

A researcher manages a set of SmartCages and mice, setting up behavior training
or experiments which will run automatically and independently on each SmartCage.
The researcher may monitor the set of SmartCages remotely or in-person and
intervene if needed, but direct supervision is not necessary for operation.

Each SmartCage contains a mouse with a headbar and consists of a typical mouse
cage with a tunnel module and task module attached. The tunnel module has
sensors for detecting the mouse position and motion and motorized latches for
head-fixing the mouse. The task module has sensors for collecting data and
actuators for interacting with the mouse.

Control and data collection in the SmartCage system is handled by a variety of
hardware and software components, communicating with each other to coordinate.
Examples of SmartCage system hardware and software components include Linux
single board computers running Python, C++, Docker, ROS 2, behavior trees, and
webservers, Phidgets sensor and actuator controllers, Windows computers running
Matlab Bpod or MacOS computers running PyBpod rodent behavior measurement and
control devices.

### ROS 2

The Robot Operating System (ROS) is a framework for splitting software into
relatively small distributed independent nodes which send messages to each
other. These nodes may all run on the same computer or on multiple computers
connected in a network. It was designed to control robots, as the name suggests.

Some people, both scientists and roboticists, feel as if it is inappropriate to
use something designed for robots on scientific instruments. Many people have a
somewhat narrow view of the forms a robot can take, thinking all robots must
either look humanoid, or like an industrial robot arm, or a drone. Many
scientific instruments, though, and especially many behavioral rigs, have
sensors, actutators, computers, and controllers, just like any other robot,
except in a rearranged form.

Using a set of small, easy-to-understand, nodes and combining them together is
akin to the Unix philosophy of making each program do one thing well and
combining small programs to make a more complex program, rather than adding
feature after feature to a single large program until it is too complicated to
understand.

ROS 2 improves upon the first version in many ways and now runs on Windows, as
well as Linux and MacOS, and works with Matlab, as well as C++, Python, Java,
and Javascript.

ROS 2 is one way of connecting heterogenous hardware and software into a larger
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

Behavior trees might be used for more than control. Perhaps software that
automatically observes and classifies behavior could generate or rearrange
behavior trees to help explain animal or other observed behaviors in an
experiment.

[behavior trees](https://arxiv.org/abs/1709.00084)
