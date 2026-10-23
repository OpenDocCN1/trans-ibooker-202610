
Manager: Our users will directly speak to our app or website and get results nicely presented.
John: Hmm, so this means that the input data is speech, and the output can be . . . anything?
Manager: Yes, the input is speech, and the output is the answer to the user’s question. We know that NLP is hard, so we thought we could reduce the complexity by reducing this application’s depth: this feature will help users query houses and nothing more. For now.
John: OK, this helps, but I think we’re talking about two different AI projects here.
Manager: What do you mean?
John: Well, we can’t directly process audio and come up with answers; there’s an intermediary step. We first need to transcribe the audio into text; this is a general application called speech to text. Then, after we have the user’s question in plain text, we must understand the intention. You can call this application text classification or intent detection. It’s doable, but must be broken into two parts.

Getting your AI project ready to be digested by a technical team might not be as straightforward as you expect. As in some of our examples, many AI-amenable problems are relatively straightforward and can be described as a single idea. For example, the home-price-prediction model is a single product. In other cases, the end goal is more complex and needs to be broken down further before we can talk about translating it into ML-friendly terms. In this case, our manager finds out that his AI project is actually the sum of two well-separated ML algorithms. Now we’ll teach you how to understand whether your AI project is a combination of several ML algorithms.

Let’s take as an example something that you may be using on a daily basis: a voice assistant like Apple’s Siri. Say that we want our personal assistant to do just two things: call people and send messages. Thanks to what you’ve learned in this book, you know that the first thing to do to tackle any ML problem is to think about the data involved. In our case, the input data is the voice of the person using the assistant, as recorded through the microphone. The output is an action taken by the phone: either sending a message or calling a certain person. Traditional software will take care of performing the action: the only thing we need to achieve is to use speech to decide which action to take, as shown in figure 7.6.

Basically, what we’re describing is a supervised classification problem: the input is audio data, and the output is a class. What would the classes be? Theoretically, you should have classes like “text Marc,” “call Marc,” “send a message to Marc,” “answer Marc,” and “call back Marc” for each person on your contacts list. It sounds a bit crazy, right? The problem is not only that we have many classes, but also that mapping audio files to classes directly is extremely hard. In fact, if you tried to remember all the applications we’ve covered in this book, there’s nothing like this.

![](img/51e9508eebbc565953ed15bd4b5bec8f_174_0.png)

Figure 7.6 Basic scheme of a voice assistant product

Let’s see what happens if we break the problem into smaller steps, using the technologies covered in this book and starting from the input data. We covered audio data in chapter 3, where we said that it’s possible to use deep learning to build speech recognition systems, transforming the audio data in text. This technology is pretty solid and mature, especially for the most popular languages like English and Spanish and for native speakers (we, the authors, still struggle sometimes to be understood by Siri with our Italian inflections). This means that we can be pretty confident that if our user says “text Marc,” it’s possible to build a system that takes that voice and transcribes it into written words.

After this step, the input data we’re dealing with is fundamentally changed: we’re now working with text data, and no longer with audio. We still need to understand whether the user wants to call or text, and who they want to communicate with. Let’s stay with our approach of breaking a problem into its core components, and focus on the first part. We want to build something that maps a written sentence to one of two actions: calling or texting.

When we covered text data in chapter 5, we extensively described how you can use ML to classify a bit of text into classes. In this case, we can train an ML model with tens of sentences and labels like this:

- “Call Marc” -> call
- “Can you send a message to Marc?” -> text
- “Answer Marc” -> text
- “Call back Marc” -> call
- “Text Marc” -> text

As you may recall from chapter 5, even if there are tens or hundreds of ways to express the same intention, word embeddings greatly simplify our job and allow us to solve this task. Here’s where we are now: we know what action the user wants to take, and we’re left with understanding which person to pick from the contacts list. This is rather easy, and not an ML task. Because we have the transcription of what the user has said, we can simply check all the words they pronounced and look through their contacts. When we find a name that matches what the user said, we know who to text or call.

As you can see, a problem that seemed impossible can become easy after it’s broken into small tasks, each solved with a combination of ML and traditional software engineering. Figure 7.7 is an architectural diagram of the complete product.

![](img/51e9508eebbc565953ed15bd4b5bec8f_176_0.png)

A voice assistant can be broken into several components, which convert the user voice to software commands.

You can follow the same process to break down even more-complex projects. For our next example, we’ll take an overwhelming case: building a self-driving car. Let’s imagine the steps that you would take as a human being to drive your car from your backyard in San Francisco to Los Angeles, and understand what a computer driver has to deal with. As you get into the driver’s seat, you’ll start breaking down the steps to get to your overall destination: get out of your garage, merge into the freeway, pick a radio station, try not to get bored for a few hours, and, finally, pull over at your friend’s driveway in LA. Before you can reach your destination, all these high-level goals must be broken down further: which streets to turn into, which lane to choose, and so forth. On top of this, you’ll have to observe traffic laws and pay attention to what other drivers are doing so as not to cause an accident.

This intuition will be helpful in starting to break down an AI system to achieve autonomous driving. Road routing doesn’t really need AI components, as we can just reuse the same navigation algorithms used in Google Maps. On the other hand, there’ll be plenty of machine learning in the perception component, which is tasked with figuring out where pedestrians, cyclists, and other cars are in relation to the vehicle itself. Because some of the most commonly used sensors in self-driving cars are cameras, the natural choice is to use deep learning for image classification and object localization models (as we explained in chapter 4). For this component, the input data is images captured from front- and side-facing cameras mounted to the car, and the outputs are the locations of nearby objects.

Understanding that cars and pedestrians are on the street is not enough; you also need to predict what other road participants are about to do, so the self-driving car can make the best and safest decisions. For example, other than knowing that pedestrians are on the sidewalk, you also want AI to help you predict whether they’re about to cross the street and slow down for safety. This ML task is a bit more esoteric than the previous one (classifying objects around the vehicle), so let’s see how we would frame it in ML terms. We’re not even sure about what we want out of the model: is it enough to have a simple classification such as “likely to move” and “not likely to move,” or will the model have to predict the direction of movement too? Both choices are valid (and a real project would probably run both), but the latter is more useful—at the cost of more complexity and training data. From the point of view of the input data, the most relevant bits are probably the location (city or highway), surroundings (street crossing, sidewalk, entrance to an office), and type of entity (pedestrian, cyclist, or vehicle).
