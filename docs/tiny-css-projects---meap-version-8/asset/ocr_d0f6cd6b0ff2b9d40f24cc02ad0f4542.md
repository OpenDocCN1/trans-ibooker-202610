+   - Block
+     - The purpose, what the block is for or doing
+     - Class name for the element such as `header`
+   - Element
+     - Describes the element’s purpose
+     - Class name is the block name followed by 2 underscores and the the element such as `header__text`
+   - Modifier
+     - Describes the appearance, state, and behavior
+     - The class pattern is as follows: Block-name_modifier-name (ex: `header_mobile`); Block-name__element-name_modifier-name (ex: `header__menu_open`)

# 11.1.4 Picking an approach

Choosing an architectural approach for CSS is very team dependent. The needs of the project, size and experience of the team, as well as the libraries and frameworks being used are all factors in how to choose which architecture to use. There is no one size fits all or universally correct answer, it is a decision that needs to be made by the team.

Because of its component based approach, for this chapter we will use BEM architecture to scope and style our social media share links.

# 11.2 Setup

Now that we have chosen our methodology which dictates the naming convention we will use for the project, let’s take a look at what we will be building. We will style a share button which when clicked opens a set of links which allow the user to share the page via email or to Facebook, LinkedIn, or Twitter. We will then use transitions to animate opening and closing the share options and the hover and focus effect of the individual links. Our end goal is shown in Figure 11.1.

![](bbox=[0.18, 0.69, 0.8, 0.75])

Figure 11.1: End goal

Our starting HTML (Listing 11.1) consists of a container to hold our component, a share button, and a menu to choose what we want to use to share the page to. We also have a linked JavaScript file, which makes our component usable via keyboard navigation and triggers showing and hiding the links inside of the component when the share button is clicked. As we will see later in this project, there are a handful of limitations to animating elements using only