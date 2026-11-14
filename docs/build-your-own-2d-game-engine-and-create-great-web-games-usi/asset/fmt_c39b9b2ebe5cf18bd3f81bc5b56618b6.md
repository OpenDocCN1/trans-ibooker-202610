
With your current particle system implementation, you can create particles at a specific point and time. These particles can move and change based on their properties. However, particles can be created only when there is an explicit state change such as a key click. This becomes restricting when it is desirable to persist the generation of particles after the state change, such as an explosion or firework that persists for a short while after the creation of a new `RigidShape` object. A particle emitter addresses this issue by defining the functionality of generating particles over a time period.

## The Particle Emitters Project

This project demonstrates how to implement a particle emitter for your particle system to support particle emission over time. You can see an example of this project running in Figure [10-3](#334805_2_En_10_Chapter.xhtml#Fig3). The source code of this project is located in the `chapter10/10.3.particle_emitters` folder. 

![Figure 10-3 ¶ Running the Particle Emitters project](images/334805_2_En_10_Chapter/334805_2_En_10_Fig3_HTML.jpg)

Figure 10-3 ¶ Running the Particle Emitters project

The controls of the project are identical to the previous project and support all of the rigid shape and collision controls. The particle system–specific controls of the project are as follows:

*   `Q key`: To spawn particles at the current mouse position
*   `E key`: To toggle the drawing of particle bounds
*   `1 key`: To toggle `Particle`/`RigidShape` collisions

The goals of the project are as follows:

*   To understand the need for particle emitters
*   To experience implementing particle emitters

### Defining the ParticleEmitter Class

You have observed and experienced the importance of avoiding patterns when working with particles. In this case, as the `ParticleEmitter` object generates new particles over time, once again, it is important to inject randomness to avoid any appearance of a pattern.

1. In the `src/engine/particles` folder, create `particle_emitter.js`; define the `ParticleEmitter` class with a constructor that receives the location, number, and how to emit new particles. Note that the `mParticleCreator` variable expects a callback function. When required, this function will be invoked to create a particle.

```javascript
let kMinToEmit = 5; // Smallest number of particle emitted per cycle
class ParticleEmitter {
    constructor(px, py, num, createrFunc) {
        // Emitter position
        this.mEmitPosition = [px, py];
        // Number of particles left to be emitted
        this.mNumRemains = num;
        // Function to create particles (user defined)
        this.mParticleCreator = createrFunc;
    }
    ... implementation to follow ...
}
export default ParticleEmitter;
```

2. Define a function to return the current status of the emitter. When there are no more particles to emit, the emitters should be removed.

```javascript
expired() {
    return (this.mNumRemains <= 0);
}
```

3. Create a function to actually create or emit particles. Take note of the randomness in the number of particles that are actually emitted and the invocation of the `mParticleCreator()` callback function. With this design, it is unlikely to encounter patterns in the number of particles that are created over time. In addition, the emitter defines only the mechanisms of how, when, and where particles will be emitted and does not define the characteristics of the created particles. The function pointed to by `mParticleCreator` is responsible for defining the actual behavior of each particle.

```javascript
emitParticles(pSet) {
    let numToEmit = 0;
    if (this.mNumRemains < this.kMinToEmit) {
        // If only a few are left, emits all of them
        numToEmit = this.mNumRemains;
    } else {
        // Otherwise, emits about 20% of what's left
        numToEmit = Math.trunc(Math.random() * 0.2 * this.mNumRemains);
    }
    // Left for future emitting.
    this.mNumRemains -= numToEmit;
    let i, p;
    for (i = 0; i < numToEmit; i++) {
        p = this.mParticleCreator(
            this.mEmitPosition[0], this.mEmitPosition[1]);
        pSet.addToSet(p);
    }
}
```

Lastly, remember to update the engine access file, `index.js`, to allow game developer access to the `ParticleEmitter` class.

### Modifying the Particle Set

The defined `ParticleEmitter` class needs to be integrated into `ParticleSet` to manage the emitted particles:

1. Edit `particle_set.js` in the `src/engine/particles` folder, and define a new variable for maintaining emitters:

```javascript
constructor() {
    super();
    this.mEmitterSet = [];
}
```

2. Define a function for instantiating a new emitter. Take note of the `func` parameter. This is the callback function that is responsible for the actual creation of individual `Particle` objects.

```javascript
addEmitterAt(x, y, n, func) {
    let e = new ParticleEmitter(x, y, n, func);
    this.mEmitterSet.push(e);
}
```

3. Modify the `update` function to loop through the emitter set to generate new particles and to remove expired emitters:

```javascript
update() {
    super.update();
    // Cleanup Particles
    let i, obj;
    for (i = 0; i < this.size(); i++) {
        obj = this.getObjectAt(i);
        if (obj.hasExpired()) {
            this.removeFromSet(obj);
        }
    }
    // Emit new particles
    for (i = 0; i < this.mEmitterSet.length; i++) {
        let e = this.mEmitterSet[i];
        e.emitParticles(this);
        if (e.expired()) {  // delete the emitter when done
            this.mEmitterSet.splice(i, 1);
        }
    }
}
```

### Testing the Particle Emitter

This is a straightforward test of the correct functioning of the `ParticleEmitter` object. The `MyGame` class `update()` function is modified to create a new `ParticleEmitter` at the position of the `RigidShape` object when the `G` or `H` key is pressed. In this way, it will appear as though an explosion has occurred when a new `RigidShape` object is created or when `RigidShape` objects are assigned new velocities.

In both cases, the `_createParticle()` function discussed in the first project of this chapter is passed as the argument for the `createrFunc` callback function parameter in the `ParticleEmitter` constructor.

### Observations

Run the project and observe the initial firework-like explosions at the locations where the initial `RigidShape` objects are created. Type the `G` key to observe the accompanied explosion in the general vicinity of the newly created `RigidShape` object. Alternatively, you can type the `H` key to apply velocities to all the shapes and observe explosion-like effects next to each `RigidShape` object. For a very rough sense of what this particle system may look like in a game, you can try enabling texturing (with the `T` key), disabling `RigidShape` drawing (with the `R` key), and typing the `H` key to apply velocities. Observe that it appears as though the `Renderable` objects are being blasted by the explosions.

Notice how each explosion persists for a short while before disappearing gradually. Compare this effect with the one resulting from a short tapping of the `Q` key, and observe that without a dedicated particle emitter, the explosion seems to have fizzled before it begins.

Similar to particles, emitters can also have drastically different characteristics for simulating different physical effects. For example, the emitter you have implemented is driven by the number of particles to create. This behavior can be easily modified to use time as the driving factor, for example, emitting an approximated number of particles over a given time period. Other potential applications of emitters can include, but are not limited to:

*   Allowing the position of the emitter to change over time, for example, attaching the emitter to the end of a rocket
*   Allowing emitter to affect the properties of the created particles, for example, changing the acceleration or velocity of all created particles to simulate wind effects
