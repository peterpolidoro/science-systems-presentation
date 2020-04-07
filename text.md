---
layout: default
---

In this time of weirdness, I will not attempt to make a formal presentation.
Instead I will write out some thoughts that I wanted to present and hopefully my
internet will work well enough to have an informal discussion about them over
video chat at the allotted meeting time. If not, then anyone can feel free to
email me any thoughts at any time: polidorp@janelia.hhmi.org

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

[object process methodology](https://link.springer.com/chapter/10.1007/978-3-642-15865-0_7)

[robot operating system version 2](https://index.ros.org/doc/ros2/)

## Background

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
me about some software his friend was working on that originated from Stanford.

The Robot Operating System (ROS) is a framework for splitting software into
relatively small distributed nodes which send messages to each other. The first
tutorial on the ROS website involves driving a simulated turtle around in a two
dimensional window, drawing lines wherever it goes. This is an homage to the
LOGO educational programming language. I was intrigued from the start since this
was exact problem I was trying to solve, driving a magnet around on a two
dimensional plate.

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
System.
