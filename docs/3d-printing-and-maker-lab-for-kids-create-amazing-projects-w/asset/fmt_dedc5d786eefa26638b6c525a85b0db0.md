
# Lab 18: Knuckle and Finger Joints

## Step 2: Create Socket
We will now use the `Socket` tool from the `Connector` toolbox to create the socket of the ball-and-socket joint of the knuckle (fig. 2 arrow identifies the socket tool). We will not resize the `Socket` tool as it is matched to the `Ball` tool for the rest of the joint.

![Image](images/f0095-02.jpg)

**Fig. 2:** *Add sockets for the knuckles.*

## Step 3: Create Ball and Fingers (Proximal Phalanx)
Create the ball and fingers (proximal phalanx) using the `Ball` tool from the `Connector` toolbox to create the ball of the ball-and-socket joint (fig. 3). We will not resize the `Ball` tool as it is matched to the `Socket` tool we created in step 2.

![Image](images/f0096-01.jpg)

**Fig. 3:** *Add balls to cylinders to create fingers.*

## Step 4: Create Fingers
To create the fingers, use the `Cylinder` tool with proportionate lengths for each finger. Match length and width to those of your own finger (fig. 3).

## Step 5: Repeat for Rest of Fingers
Repeat steps 3 and 4 to create the rest of the fingers as shown in fig. 4. Add a knuckle (see step 2) to the other end of each finger as seen in fig. 4. The proximal phalanx will attach to this knuckle.

![Image](images/f0096-02.jpg)

**Fig. 4:** *Add sockets for the knuckle joints to the finger at the opposite end from the ball joints.*

## Step 6: Create Middle Phalanx
Create the middle phalanx by repeating steps 2–4 to create the middle phalanx and matching ball of the knuckle (fig. 5, #1).

![Image](images/f0096-03.jpg)

**Fig. 5:** *Add a fingertip to each finger.*

## Step 7: Add Fingertip
Add the fingertip using the `Paraboloid` tool as shown in fig. 5, arrow 2. Repeat step 5 and 6 for all fingers.

## 3D Printing Checklists

### Before Printing Your CAD
![Images](images/square.jpg)   See 3D Printing Basics shown [`#page_9`](#page_9) for printing tips.

![Images](images/square.jpg)   Print the hand.

![Images](images/square.jpg)   After printing, remove any excess material with a file.

### Quality Control & Product Testing
![Images](images/square.jpg)   Assemble all the knuckles by putting balls into sockets (fig. 6).

![Image](images/f0097-01.jpg)

**Fig. 6:** *The CAD view of the assembled hand with fingers and palm.*

![Images](images/square.jpg)   Test the product for its ability to curve its fingers.

## Think About It
Consider this question and make notes about possible answers. You can test them by modifying and printing a new version of your design.

**How would using the ball-and-socket joint designed in Lab 15 change how the finger moves?**

---

# Lab 19: Human Arm

| Lab | Body System |
|------|-------------|
| 19   | Human Arm   |

For this Lab we will extend our design skills of complex human body parts to design a human arm. Our model is simplified with only one forearm bone instead of two, and with the hand as one single piece.

![Image](images/f0098-01.jpg)

## The Design Process

### Step 1: Create Shoulder Joint
Create the shoulder joint. The shoulder joint is modeled using the ball-and-socket joint we created in Lab 15 (shown [`#page_80`](#page_80)). The socket inner diameter is 23 mm and the ball is 22 mm diameter in fig. 1.

![Image](images/f0099-01.jpg)

**Fig. 1:** *Create a model of the shoulder using a ball-and-socket joint.*

### Step 2: Create Upper Arm
We will model the upper arm as a tapered tubular shape. We used the `High Resolution Tube` shape generator tool. See fig. 2, arrow 1 for details and dimensions.

![Image](images/f0099-02.jpg)

**Fig. 2:** *Make the upper arm using a* `High Resolution Tube`.

### Step 3: Create Elbow Joint
The elbow joint can be modeled using the same ball-and-socket joint we used for the shoulder joint in step 1. We can simply make a copy of it and resize to fit the smaller end of the upper arm as shown in fig. 1. The inner diameter of the elbow joint socket is 18 mm. The diameter of the elbow joint ball is 17 mm.

### Step 4: Create Forearm
The forearm is modeled using the same tool as the upper arm (fig. 3). The dimensions of the larger end must match the dimensions of the ball in the elbow joint.

![Image](images/f0100-01.jpg)

**Fig. 3:** *Copy the upper arm and resize it to create the forearm.*

### Step 5: Create Wrist
The wrist can be modeled using the ball-and-socket joint as well. Socket dimensions must match the narrower end of the forearm.

### Step 6: Create Hand
The hand connects to the wrist using the wrist joint. The hand shown in fig. 4 is modeled after the hand in Lab 18 (shown [`#page_94`](#page_94)), but as a single piece without joints.

![Image](images/f0100-02.jpg)

**Fig. 4:** *Make a simple hand form with no joints.*

### Step 7: Reposition Parts for Printing
Prior to printing, we will need to reposition some of the parts. The sockets of each joint attach to the part above it, and the ball attaches to the part following it in the sequence of parts of the arm. See fig. 5 for the print layout.

![Image](images/f0100-03.jpg)

**Fig. 5:** *The positioning of parts for 3D printing.*

## 3D Printing Checklists

### Before Printing Your CAD
![Images](images/square.jpg)   See 3D Printing Basics shown [`#page_9`](#page_9) for printing tips.

![Images](images/square.jpg)   Print the arm.

![Images](images/square.jpg)   After printing, remove any excess material with a file. Remove any rafts or supports.

### Quality Control & Product Testing
![Images](images/square.jpg)   Assemble all the parts (fig. 6). Test the product for its range of motion.

![Image](images/f0101-01.jpg)

**Fig. 6:** *A CAD Image showing the assembled arm with joints.*

## Think About It
Consider this question and make notes about possible answers. You can test them by modifying and printing a new version of your design.

**How would using the ball-and-socket joint designed in Lab 18 for the knuckles change how the shoulder moves?**

---

# Lab 20: Mini Guitars with Multiple String Types

| Lab | Topic |
|------|-------|
| 20   | Mini Guitars with Multiple String Types |

In this Lab we learn how various materials can interact with the same structure to produce different sound. We will design a mini-guitar and test the sound using different materials for the guitar strings.

![Image](images/f0102-01.jpg)

## The Design Process

### Creating the Body

#### Step 1: Design Guitar Body
The guitar body is a nonbasic shape and is hard to model using a composite of basic shapes. We will use the `Scribble` tool from the `Basic Shapes` toolbox to design the guitar body. Fig. 1 shows how to use the `Scribble` tool to design a rough 3D solid guitar body. Arrow 1 in the image identifies the tool to use.

![Image](images/f0103-01.jpg)

**Fig. 1:** *Use the* `Scribble` *tool to design the guitar body.*

#### Step 2: Smooth Body
The body created in step 1 looks rough. It is smoothed out using the `Erase` tool as shown in fig. 2. Arrow 1 in the image identifies the `Erase` tool, and arrow 2 shows how it is applied to smooth the edge of the guitar body. Click the `Done` button to complete editing the shape. Fig. 3 shows the resulting 3D guitar body after initial smoothing of the body. Keep editing the body by smoothing the edges to get the desired shape of the guitar body. Fig. 4 shows a comparison between initial smoothing and final guitar body.

![Image](images/f0104-01.jpg)

**Fig. 2:** *Use the erase function to smooth out the scribble.*

![Image](images/f0104-02.jpg)

**Fig. 3:** *The guitar body shape after initial smoothing.*

![Image](images/f0104-03.jpg)

**Fig. 4:** *The comparison between the initial guitar shape and final shape after smoothing is complete.*

#### Step 3: Create Sound Hole
The sound hole is created by making a cylindrical hole in the center of the top face of the guitar body. The hole must be 2 mm from the bottom surface of the guitar. See fig. 5 for sound hole dimensions and location.

![Image](images/f0105-01.jpg)

**Fig. 5:** *Make the sound hole in the guitar body.*
