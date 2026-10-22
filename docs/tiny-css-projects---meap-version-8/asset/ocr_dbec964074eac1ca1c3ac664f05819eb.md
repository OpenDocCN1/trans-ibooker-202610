To change the background color for Firefox, we add a background property value to the progress element itself. We will use the same gradient we used in the ::-webkit-progress-bar rule. Figure 3.20 shows our progress in Firefox.

![](bbox=[0.18, 0.2, 0.9, 0.38])

Figure 3.20 Firefox with background applied to the progress element

The last thing we need to do is remove the border, which we will apply to the progress rule. To achieve this we set the border property value to none. Our final progress rule is shown in Listing 3.17.

# Listing 3.19 Finalized progress rule

```
progress {
  height: 24px;
  width: 100%;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  border-radius: 20px;
  background: linear-gradient(to right, #128688 0%,#4db3ff 100%);  #A
  border: none;                                           #B
}
```

#A Gradient background
#B Removing the border

As we can see in Figure 3.21, we have achieved the same look in Chrome and in Firefox.