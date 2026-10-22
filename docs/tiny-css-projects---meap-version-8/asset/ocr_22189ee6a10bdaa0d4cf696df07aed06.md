![](bbox=[0.18, 0.14, 0.69, 0.32])

Figure 11.11: Share link hover effect

# 11.6 Animating the component

In Chapter 2 we used keyframes to create animation. This allowed us to define steps for our animation. For our hover we already have our start and end states defined. We are transitioning from one state (not hovered or focused) to another (hovered or focused), whose styles are already defined in rules so instead of using an animation we are going to use a transition.

# 11.6.1 Transition

A transition does not require a keyframe, and allows us to animate the change of styles from one state to another. The transition property allows us to define which property changes should be animated, the duration, and timing function. By adding `transition: transform ease-in-out 250ms;` to our `.share__link` rules we tell the browser to animate the size change of our link (listing 11.10).

To choose the amount of time the transition needs to take, we choose something relatively fast: 250 milliseconds. We want to keep the animation slow enough to be seen but fast enough to keep it snappy. If we make the transition too slow it will make our project look laggy and distract the user from performing the task they are trying to accomplish (share the content).