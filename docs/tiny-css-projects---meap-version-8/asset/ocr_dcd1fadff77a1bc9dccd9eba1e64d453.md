downloads. Table 11.1 lists the social media platforms we included in our component and links to their brand information.

# Table 11.1: Social media brand resources

| Brand | Icon | Link to assets |
|---|---|---|
| Facebook |  | https://www.facebook.com/brand/resources/facebookapp/logo/ |
| LinkedIn |  | https://brand.linkedin.com/downloads |
| Twitter |  | https://about.twitter.com/en/who-we-are/brand-toolkit |

For this project, we sourced our social media icons directly from the respective brand guides.

# 11.3.2 Icon libraries

Looking for individual icons can be a bit tedious especially on large projects, so it is not uncommon to use icon fonts and libraries to supply us with our icons. As mentioned above, these are subject to terms of use. Each library and icon font will have their own rule as to where and how they can be used. Some may also require attribution, we therefore must take care to be aware of any rules we need to follow while sourcing our icons.

For this project, we sourced our non brand related icons from Material Symbols (https://fonts.google.com/icons). Since we only needed 2 (share - and email -@), we downloaded the individual SVGs and included them in our icon folder rather than importing the entire library into the project.

The icons have already been provided in the starter code, so we are ready to start styling.

# 11.4 Styling the block

Since we are using BEM for our naming convention, our block name will be "share". Therefore the container `div` that wraps the entire component will have a class of `share`. This block name will also be included in all of our future classes using the BEM naming convention mentioned in section 11.1.3, which will scope our CSS to that component and help prevent any styling collisions between our component and any other parts of the application it would be used in.

As seen in listing 11.2, we define the `font-family`, `background`, and `border-radius`, for the block. We also give the component a `display` value of `inline-flex`. `inline-flex` works the same way as `flex` but makes the element an inline-level element rather than a block-level element. By making our component behave as an inline element (same as links, spans, buttons... ), we give it the most versatility in terms of placement when it is included into an