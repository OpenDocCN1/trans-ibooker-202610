

background images use the `linear-gradient()` function, not the `url()` function, therefore, our value is being replaced with a value of `none`.

To remove the system provided arrow, we gave the dropdown an `appearance` property value of `none`. To reinstate the system provided appearance in `forced-colors` mode, inside of the media query, we will give the `select` of an `appearance` property value of `auto`. Our media full media query is displayed in Listing 10.31.

## Listing 10.31 forced-colors: active media query

```
@media (forced-colors: active) {

  :where(input[type="radio"], input[type="checkbox"]):checked::before {
    background-color: CanvasText;
  }

  select {
    appearance: auto;
  }
}
```

Figure 10.41 shows our page in forced-colors mode with our media query applied which fixes the styles that were creating usability problems for our users.

## Contact

Your opinion is important to us. Please fill out all of the fields.

Your Name
Jane Doe

Your Email Address
jane@email.co

Reason For Contact
Sales inquiry

Are you currently a subscriber?
Yes
No

Message
Error: Please let us know how we can help

Subscribe to our newsletter

SEND
reset

Figure 10.43 forced-colors: active styles fixed

When we turn the emulation off, we can see in figure 10.41 that our previously set styles remain as they were and were not affected by those set inside of the media query.

![](img/b56145aa9b751926a7aedbd7172bc818_301_0.png)

Figure 10.44 Finished Product

With this last task complete, we have now finished styling our form.

## 10.10 Summary

- Form controls whose functionality is tightly coupled with the operating system such as dropdowns are harder to style than those without this coupling
- We can create shapes using gradients
- Using em, elements can be sized to scale with text size
- To inherit font color when otherwise not possible we can use the keyword value currentColor
- :where() and :is() pseudo classes work very similarly but have different levels of specificity
- The :checked allows us to target form elements when they are checked
- The :invalid pseudo class can be used to conditionally format fields when they are invalid
- The validity of a field's value is determined by the attributes set on the field in the HTML
- :focus styles are necessary to make our designs accessible
- We can use :focus-visible to make focus style only show for keyboard users
- In some browsers we can forcibly make the browser apply hover and focus styles
- It is important to always use more than just color to convey meaning
- forced-colors mode changes how some properties behave and the colors we can apply to the UI
- Media queries can be used to conditionally apply styles when forced-colors is set to

active

- In some browsers we can emulate `forced-colors` mode to check our designs

## 11

## Animated social media share links

## This chapter covers

- OOCS, SMACSS, and BEM Architecture Patterns
- Scoping CSS when working with components
- Working with social media icons
- Creating CSS transitions
- Where to use JavaScript to overcome CSS limitations

One of the core foundational reasons for which the internet was created was to share and distribute information. One of the ways we do this today is through social media. In this chapter we will style and animate some links that can be used to share a web page via email or social media.

Like in the previous chapters, we will be using HTML and CSS for this project without using any frameworks. This is to keep focus on the CSS itself without the complexity and individual intricacies that come with using external packages. However, many applications in the wild do use frameworks some of which include the concept of the component. A very common reason to turn a piece of functionality into a component is to reuse the piece of code or element in multiple places in our applications. With reusability comes the possibility for naming collisions. Some systems automatically restrict the scope of the CSS of the component to itself, preventing any possible collision between component styles. However, many do not, leaving it up to the developer to organize their code to prevent accidentally changing the styles in another component when styling a new one. Regardless of the framework and how it does (or does not) handle CSS scoping we have a variety of architecture options to help us organize and standardize our styles.

Before we dive into the project itself let’s take a quick look at some CSS architecture options and choose one for our project.

## 11.1 CSS Architecture

Some of the most popular CSS architecture methodologies include OOCSS, SMACSS, and BEM.

### 11.1.1 OOCSS

https://github.com/stubbornella/oocss/wiki

First presented at Web Directions North in Denver by Nicolle Sullivan, OOCSS stands for Object Oriented Cascading Style Sheets and aims to help developers create CSS that is fast, maintainable, and standards-based. The Object in Object Oriented Cascading Style Sheet is described by Sullivan as

> "a repeating visual pattern, that can be abstracted into an independent snippet of HTML, CSS, and possibly JavaScript. That object can then be reused throughout a site¹"

In other words, what we might think of today as a component or widget. To achieve this reusability, OOCSS follows 2 main principles.

- **Separate structure and skin**: keeping visual features (background, borders... sometimes referred to as theme) as their own classes that can be mixed and matched with objects to create a variety of elements
- **Separate container and content**: By refraining from using location dependent styles, we can ensure the objects look the same no matter where they are placed in the application or site.

### 11.1.2 SMACSS

http://smacss.com

Developed by Jonathan Snook, SMACSS stands for Scalable and Modular Architecture For CSS. It aims to help organize CSS by organizing rules into 5 categories.

- **Base**: the defaults applied by using element, descendent, or child selectors and pseudo classes.
- **Layout**: used to lay elements out on the page such as headers, articles, and footers
- **Module**: More discrete parts of the layout such as carousels, cards, nav bars, etc.
- **State**: something that augments or overrides other styles such as an error state or whether a menu is opened or closed.
- **Theme**: defines the looks and feel. The theme does not always have to be separated in its own classes if there is only one theme for the page or project.

### 11.1.3 BEM

https://en.bem.info/methodology/

Developed by a company named Yandex, BEM stands for Block Element Modifier. It is a component based architecture which aims to break the UI into independent blocks which can be reused.

- Block
  - The purpose, what the block is for or doing
  - Class name for the element such as `header`
- Element
  - Describes the element’s purpose
  - Class name is the block name followed by 2 underscores and the the element such as `header__text`
- Modifier
  - Describes the appearance, state, and behavior
  - The class pattern is as follows: Block-name_modifier-name (ex: `header_mobile`); Block-name__element-name_modifier-name (ex: `header__menu_open`)

### 11.1.4 Picking an approach

Choosing an architectural approach for CSS is very team dependent. The needs of the project, size and experience of the team, as well as the libraries and frameworks being used are all factors in how to choose which architecture to use. There is no one size fits all or universally correct answer, it is a decision that needs to be made by the team.

Because of its component based approach, for this chapter we will use BEM architecture to scope and style our social media share links.

## 11.2 Setup

Now that we have chosen our methodology which dictates the naming convention we will use for the project, let’s take a look at what we will be building. We will style a share button which when clicked opens a set of links which allow the user to share the page via email or to Facebook, LinkedIn, or Twitter. We will then use transitions to animate opening and closing the share options and the hover and focus effect of the individual links. Our end goal is shown in Figure 11.1.

![](img/b56145aa9b751926a7aedbd7172bc818_305_0.png)

Figure 11.1: End goal

Our starting HTML (Listing 11.1) consists of a container to hold our component, a share button, and a menu to choose what we want to use to share the page to. We also have a linked JavaScript file, which makes our component usable via keyboard navigation and triggers showing and hiding the links inside of the component when the share button is clicked. As we will see later in this project, there are a handful of limitations to animating elements using only

