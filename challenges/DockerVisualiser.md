## Task ID: DockerVisualiser

#### `Full Stack Web Development`, `Docker`, `UI/UX`, `Three.js`

Mentors: [Varshini Adurti](https://github.com/VarshiniAdurti28) ([+91 9632079916](https://wa.me/919632079916))

Difficulty: `Medium-Hard`

### Description

The task is to build an interactive learning environment that helps developers understand Docker by **seeing what happens when Docker commands are executed**.

The application should combine a **command-line interface** with an **animated visual playground**.

The idea is if a user enters Docker commands in a terminal, the corresponding Docker state is represented visually in the playground.

The task **does not require you to run a real Docker daemon**. Parse each command in your own code and play the result as an animation. Do not pass the typed string to a shell or to a real Docker daemon.

---

For example:

```bash
docker run ubuntu
```

should result in a simulated container being created from the `ubuntu` image and that change should be visible in the playground.
This should also include the playground showing: 
- looking up for 'ubuntu' in the list of local host images, 
- not finding it (on the first run), pulling the image, and spinning up the container finally. 
- However, on the subsequent runs of the command, the visualisation shows Docker creating container directly.
  
**Do not** just display text representations of the commands or flowcharts.

---

# Scope

The task consists of **three versions**. Start with the first, test rigorously and then move on to the next version.

## Version 1: Containers

Implement a basic simulation of Docker containers.

The user should be able to perform operations such as:

```bash
docker run <image>
docker start <container>
docker stop <container>
docker rm <container>
```

Creating, stopping, starting, and removing a container should result in appropriate visual state changes.

The exact visual representation and animations are left to you.

**Note:** State should be consistent, for example, all the above commands should run for the same container (for a given container id).

And the execution of the next command should be dependent on the state.

For example:

```bash
docker rm <container>
docker start <container>
```

As the container no longer exists, the command 'docker start' should fail.

In any other case of failure, either the command successfully produces its complete state transition, or it fails completely without leaving the environment in a partially modified state.

---

## Version 2: Images and Layers

Extend the simulation to represent Docker images and their layers.

The application should support concepts such as:

```bash
docker pull <image>
docker build -t <name> .
```

A simplified Dockerfile model should also be supported.

For example:

```dockerfile
FROM ubuntu
RUN apt install nginx
COPY app.js /app/
CMD ["node", "app.js"]
```

The application should be able to represent the relationship:

```text
Dockerfile
    ↓
Instructions
    ↓
Image layers
    ↓
Image
    ↓
Container
```

The visualization should make these relationships understandable.

---

## Version 3: Container Filesystem and Processes

Extend containers to contain a simplified filesystem and process model.

A container may contain objects such as:

```text
/
├── bin/
├── etc/
├── home/
└── tmp/
```

and processes such as:

```text
PID 1
bash
```
The visualisation SHOULD show the above.

Support commands such as:

```bash
docker exec <container> ...
```

and basic filesystem operations where appropriate.

For example, running a command that creates a file inside a container should result in that file appearing in the simulated container filesystem.

---

# User Interface

The application should have two primary areas.

### Terminal

A command-line interface where the user enters supported Docker commands.

It should provide useful messages for:

* valid commands
* invalid commands
* missing images
* missing containers
* invalid operations

### Visualization Playground

An interactive animated visualization representing the current simulated Docker environment.

The exact visual style, design and animations are left open to you.

---

# Evaluation

The implementation will be evaluated primarily on:

* Correctness of the simulated Docker behavior
* Consistency of state across commands
* Quality of the visualization
* Clarity of the Docker concepts being represented
* Quality of the user interaction
* Code organization and maintainability
* Handling of invalid or unexpected commands
* Creativity of the visual representation


# Some additional points

* A complete submission includes all three versions (containers, images and layers, filesystem and processes)
* The more commands you can include, the better
* Try and include a diverse set of commands in the implementation rather than wrapping up with just the basic commands (which doesn't mean you jump to the advanced commands directly!)

---

# A Few Starting Commands:

You could start with commands such as:

```bash
docker pull ubuntu
docker run ubuntu
docker start <container>
docker stop <container>
docker rm <container>
```

and then introduce:

```bash
docker build -t <name> .
docker exec <container> <command>
```

# Deployment

The playground must be on a public URL. The GitHub repo stays private.

# Resources:
* [Docker tutorial](https://www.docker.com/101-tutorial/)
* [Docker CLI reference](https://docs.docker.com/reference/cli/docker/)
* [Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
* [Three.js and React three fiber ref 1](https://www.smashingmagazine.com/2020/11/threejs-react-three-fiber/)
* [Three.js and React three fiber ref 2](https://jsmastery.com/blogs/the-ultimate-guide-to-mastering-three-js-for-3d-development)
