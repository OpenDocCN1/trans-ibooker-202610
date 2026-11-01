
3. Choose the Eyedropper Tool (I) or the fill icon:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_135_0.png)

Eyedropper

4. Once you have selected either one of these, you can change the color of the element:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_136_0.png)

Changing the color of an element

Changing the color of text is done just as it is in After Effects. You can use the Eyedropper tool to choose a color onscreen to change your element to.

5. To save your file, go up to the upper-left corner of the screen and, under the File tab, choose Save As:...

![](img/fc2cc655ef17110f454c8cd7f3d9c134_137_0.png)

Save

This is where you can save your file in a variety of different formats. For our purposes, we want our illustrations saved as Illustrator files. Choose the Adobe Illustrator *AI file format.

Now jump back to After Effects. Once you import your Illustrator file into After Effects, you can jump back into Illustrator, modify the existing layers, save them, and they will automatically update in After Effects. However, any new layers created in the Illustrator file will not import into After Effects. If you need to add a new layer to an Illustrator file, I recommend you save a new Illustrator file. Import the new Illustrator file and incorporate that new layer into the After Effects project. Now we are ready to import our Illustrator files into After Effects and start animating them! I love this process because Illustrator files are so beautiful and easy to work with. So, the way we import our Illustrator files correctly is by choosing the following options:

- 1. Import As: Composition - Retain Layer Sizes
- 2. Import

Click on Create a composition.

Now that you have imported your Illustrator files, you should see a composition automatically appear in the project panel. Move it into your precomp folder:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_138_0.png)

Importing an Illustrator file

This will also import a folder with the individual layers. Move this folder into the `IMAGES` folder. I love this—it's so organized if you set it up correctly in Illustrator! Take a look at the size of these `IMAGES` in the folder. This is important because bigger layers increase the render time. Smaller-sized layers are easier to animate. Since these are Illustrator files, they can be scaled up and not lose their quality:

- 1. Navigate to the Illustrator precomp.
- 2. Duplicate this so that you have a backup copy in After Effects.
- 3. Double-click on one of these **PRECOMPS**. You will see your laptop's individual elements set up exactly how we created them in Illustrator.
- 4. When you import Illustrator files, be sure to turn on continuously rasterize:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_139_0.png)

Continuously rasterize

Continuously rasterize is a way to make your layers render in full resolution. Since these are Illustrator files, they will look crisp no matter how much they are scaled up. Now we are going to parent all of our elements to our monitor:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_140_0.png)

Parent elements

Copy all of these elements and paste them into the composition with your type composition, then do the following:

- 1. Name this composition `ONLINE LEARNING`
- 2. Select the monitor layer you parented everything to
- 3. Scale this layer down to fit in the scene

When you move this layer, all your elements will move with it, because you parented them to it:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_140_1.png)

Monitor resize

Let's take a look at what we have created so far and be proud of ourselves! We now have our first logo element set up and ready to be animated. Great job!

## Animation principles

At this point, I am going to slow things down and talk about animation principles. We are about to animate our logo and I want you to have a greater understanding of how to move your elements in convincing dynamic ways. Walt Disney — one of the great pioneers of animation — created 12 animation principles that we still use today:

- Timing
- Ease in and out (or slow in and slow out)
- Arcs
- Anticipation
- Exaggeration
- Squash and stretch
- Secondary action
- Follow through and overlapping action
- Straight ahead action and pose-to-pose action
- Staging
- Appeal
- Personality

The ones I want to pay special attention to are timing, ease in and out, secondary action, and personality. Timing is everything in animation. I will repeat this many times in this book. A rule I almost always use is the rule of 10 key frames. It takes the eye about 10 key frames to visually register anything. Make sure you have at least 10 frames between each action. You may need more frames if you have a lot of information on the screen, but this is a good starting point. Take special care in deciding how long something should be onscreen. You do not want the viewer to be bored, so do not let your animations lag. This is something you will get the hang of the more you animate.

The next thing to pay attention to is ease in and out. This is one of the most important things in animating a convincing, elegant action. In the real world, objects need time to accelerate and slow down, like the human body does; most objects move in this way. There's always a gradual easing at the start and end of a movement. To accomplish this in After Effects, we use the Easy Ease key frame assistant. Adding this immediately makes your animation look better.

Secondary actions also add some personality and realism to your animation. Secondary actions are when your objects move into their positions and have a secondary movement after they land there. It isn't necessary to always use secondary movements, but it can add weight and believability to your movements. It also makes your animations imperfect, which generates interest and creates personality. We will go over these principles a lot more in this book.

## Animating a monitor

Now let's get to animating our computer monitor:

- Move 10-20 key frames down the timeline after our text appears on the screen
- Move the anchor point of the monitor to the bottom edge of your monitor

To move your anchor point, choose the pan behind tool (Y) and move the anchor point. Now this object will animate around the anchor point. It's important to set this up before animating your object:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_142_0.png)

Monitor anchor point

At this point, we need to lock our parameters where they are. We do that by creating blank key frames:

- Set scale and rotate key frames at this point
- Move back in the timeline (to the left) 10 key frames

- Scale down the monitor to 0
- Turn on motion blur for everything

Now we are going to create a fake camera move. This will be used as a transition element for our infographic video. We create camera nulls by parenting everything but the background of the null object:

- Go to the point in the timeline after the monitor has been animated.
- Create a null object. Select everything except the background.
- Parent everything selected to the null object.
- Easy ease all of these key frames.
- Turn on motion blur for everything except the background. Remember, motion blur must be turned on in two locations:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_143_0.png)

Motion blur to icons

Easy Ease will soften your movements and will make them look more organic.
Select the key frames for the monitor and the null. Go to **Animation** | **Keyframe Assistance** | **Easy Ease**:

![](img/fc2cc655ef17110f454c8cd7f3d9c134_144_0.png)

Easy Ease

Now let's animate our fake camera push on our monitor. Here are the steps:

- 1. Stay at the point where the monitor has been animated.
- 2. Create a key frame for the scale and position of the null object.
- 3. Jump down (to the right) 10 key frames.
