# Hotels

# Meeru Island Resort & Spa

The stylish Meeru Island Resort & Spa strikes the perfect note for a peaceful escape.

Learn more

# Flamingo Beach

Plenty of pools and activities to keep children smiling, plus comfy apartments that are tailored for families.

Learn more

# Protur Safari Park

The big, family-friendly complex dishes up plenty of pools and activities.

Learn more

# Mountain View Resort

There are heaps of spots to soak in at the super-sized Holiday Village Costa del Sol.

Learn more

Figure 5.9 Layout Achieved Viewed On a 1000 Pixel wide screen

With our layout in hand, let’s start focusing on styling our content starting with the header. We are going to change the font for our h1 element and look at how to use an image to color our text.

# 5.3 Styling the header using background-clip

The title for this page—“hotels”—could be visually more interesting. One way to liven it up could be to set the color to a nice vibrant color and update the font family to something modern. Another way is to apply a background image to the text. This is possible through 2 experimental properties, the background-clip and the text-fill-color.

They are experimental because they still require vendor prefixes to support all main browsers. This can present some risk as experimental properties may have non-standard implementation and are subject to change. We can reduce this risk by setting a fallback color value so that if these 2 properties do not work the user will still see the text, just without the background image.

# Experimental Properties

Experimental properties are not recommended for production use and often have non standard implementations. For more details about experimental properties please refer to Chapter 3, Section 7: Vendor Prefixes.

# 5.3.1 Setting the font

The first step we need to take is to update the font-family, weight, and size as well as transforming the text into uppercase as seen in Listing 5.7.