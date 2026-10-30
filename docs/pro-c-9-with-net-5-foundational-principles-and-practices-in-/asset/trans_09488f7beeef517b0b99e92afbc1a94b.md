

With this update, you can now create buttons of various colors and textual values. Figure 27-13 shows the result of the updated XAML.

![](img/587021f4d5ba039be70a11107130377b_1173_0.png)

Figure 27-13. Template bindings allow values to pass through to the internal controls.

## The Role of ContentPresenter

When you designed your template, you used a Label to display the textual value of the control. Like the Button, the Label supports a Content property. Therefore, given your use of `{TemplateBinding}`, you could define a Button that contains complex content beyond that of a simple string.

However, what if you need to pass in complex content to a template member that does not have a Content property? When you want to define a generalized content display area in a template, you can use the ContentPresenter class as opposed to a specific type of control (Label or TextBlock). There is no need to do so for this example; however, here is some simple markup that illustrates how you could build a custom template that uses ContentPresenter to show the value of the Content property of the control using the template:

```xml
<!-- This button template will display whatever is set to the Content of the hosting button. -->
<ControlTemplate x:Key="NewRoundButtonTemplate" TargetType="Button">
    <Grid>
        <Ellipse Fill="{TemplateBinding Background}"/>
        <ContentPresenter HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</ControlTemplate>
```

## Incorporating Templates into Styles

Currently, your template simply defines a basic look and feel of the Button control. However, the process of establishing the basic properties of the control (content, font size, font weight, etc.) is the responsibility of the Button itself.

```xml
<!-- Currently the Button must set basic property values, not the template. -->
<Button x:Name="myButton" Foreground="Black" FontSize="20"
    FontWeight="Bold"
    Template="{StaticResource RoundButtonTemplate}"
    Click="myButton_Click"/>
```

If you want, you could establish these values in the template. By doing so, you can effectively create a default look and feel. As you might have already realized, this is a job for WPF styles. When you build a style (to account for basic property settings), you can define a template within the style! Here is your updated application resource in the application resources in App.xaml, which has been rekeyed as RoundButtonStyle:

```xml
<!-- A style containing a template. -->
<Style x:Key="RoundButtonStyle" TargetType="Button">
    <Setter Property="Foreground" Value="Black"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="FontWeight" Value="Bold"/>
    <Setter Property="Width" Value="100"/>
    <Setter Property="Height" Value="100"/>
    <!-- Here is the template! -->
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <!-- Control template from above example -->
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

With this update, you can now create button controls by setting the Style property as so:

```xml
<Button x:Name="myButton" Background="Red" Content="Howdy!" Click="myButton_Click" Style="{StaticResource RoundButtonStyle}"/>
```

While the rendering and behavior of the button are identical, the benefit of nesting templates within styles is that you can provide a canned set of values for common properties. That wraps up your look at how to use Visual Studio and the trigger framework to build custom templates for a control. While there is still much more about the Windows Presentation Foundation API than has been examined here, you should be in a solid position for further study.

## Summary

The first part of this chapter examined the resource management system of WPF. You began by looking at how to work with binary resources, and then you examined the role of object resources. As you learned, object resources are named blobs of XAML that can be stored at various locations to reuse content.

Next, you learned about WPF’s animation framework. Here you had a chance to create some animations using C# code, as well as with XAML. You learned that if you define an animation in markup, you use `<Storyboard>` elements and triggers to control execution. You then looked at the WPF-style mechanism, which makes heavy use of graphics, object resources, and animations.

You examined the relationship between a logical tree and a visual tree. The logical tree is basically a one-to-one correspondence of the markup you author to describe a WPF root element. Behind this logical tree is a much deeper visual tree that contains detailed rendering instructions.

The role of a default template was then examined. Remember, when you are building custom templates, you are essentially ripping out all (or part) of a control’s visual tree and replacing it with your own custom implementation.

## WPF Notifications, Validations, Commands, and MVVM

This chapter will conclude your investigation of the WPF programming model by covering the capabilities that support the Model-View-ViewModel (MVVM) pattern. The first section covers the Model-View-ViewModel pattern. Next, you learn about the WPF notification system and its implementation of the Observable pattern through observable models and observable collections. Having the data in the UI accurately portray the current state of the data automatically improves the user experience significantly and reduces the manual coding required in older technologies (such as WinForms) to achieve the same result.

Building on the Observable pattern, you will examine the mechanisms to add validation into your application. Validation is a vital part of any application—not only letting the user know that something is wrong but also letting them know what is wrong. To inform the user what the error is, you will also learn how to incorporate validation into the view markup.

Next, you will take a deeper dive into the WPF command system and create custom commands to encapsulate program logic, much as you did in Chapter 25 with the built-in commands. There are several advantages to creating custom commands, including (but not limited to) enabling code reuse, logic encapsulation, and separation of concerns.

Finally, you will bring all of this together in a sample MVVM application.

### Introducing Model-View-ViewModel

Before you dive into notifications, validations, and commands in WPF, it would be good to understand the end goal of this chapter, which is the Model-View-ViewModel pattern (MVVM). Derived from Martin Fowler’s Presentation Model pattern, MVVM leverages XAML-specific capabilities, discussed in this chapter, to make your WPF development faster and cleaner. The name itself describes the main components of the pattern: model, view, view model.

### The Model

The model is the object representation of your data. In MVVM, models are conceptually the same as the models from your data access layer (DAL). Sometimes they are the same physical class, but there is no requirement for this. As you read this chapter, you will learn how to decide whether you can use your DAL models or whether you need to create new ones.

Models typically take advantage of the built-in (or custom) validations through data annotations and the INotifyDataErrorInfo interface and are configured as observable to tie into the WPF notification system. You will see all of this later in this chapter.

### The View

The view is the UI of the application, and it is designed to be very lightweight. Think of the menu board at a drive-thru restaurant. The board displays menu items and prices, and it has a mechanism so the user can communicate with the back-end systems. However, there isn’t any intelligence built into the board, unless it is specifically user interface logic, such as turning on the lights if it gets dark.

MVVM Views should be developed with the same goals in mind. Any intelligence should be built into the application elsewhere. The only code in the code-behind file (e.g., MainWindow.xaml.cs) should be directly related to manipulating the UI. It should not be based on business rules or anything that needs to be persisted for future use. While not a main goal of MVVM, well-developed MVVM applications typically have very little code in the code-behind.

### The View Model

In WPF and other XAML technologies, the view model serves two purposes.

- The view model provides a single stop for all the data needed by the view. This doesn’t mean the view model is responsible for getting the actual data; instead, it is merely a transport mechanism to move the data from the data store to the view. Usually, there is a one-to-one correlation between views and view models, but architectural differences exist, and your mileage may vary.
- The second job is to act as the controller for the view. Just like the menu board, the view model takes direction from the user and relays that call to the relevant code to make sure the proper actions are taken. Quite often this code is in the form of custom commands.

### Anemic Models or Anemic View Models

In the early days of WPF, when developers were still working out how best to implement the MVVM pattern, there were significant (and sometimes heated) discussions about where to implement items like validation and the Observable pattern. One camp (the anemic model camp) argued that it all should be in the view model since adding those capabilities to the model broke separation of concerns. The other camp (the anemic view model camp) argued it should all be in the models since that reduced duplication of code.

The real answer is, of course, it depends. When INotifyPropertyChanged, IDataErrorInfo, and INotifyDataErrorInfo are implemented on the model classes, this ensures that the relevant code is close to the target of the code (as you will see in this chapter) and is implemented only once for each model. That being said, there are times when your view model classes will need to be developed as observables themselves. At the end of the day, you need to determine what makes the most sense for your application, without over-complicating your code or sacrificing the benefits of MVVM.

> **Note:** There are multiple MVVM frameworks available for WPF, such as MVVMLite, Caliburn.Micro, and Prism (although Prism is much more than just an MVVM framework). This chapter discusses the MVVM pattern and the features in WPF that support implementing the pattern. I leave it to you, the reader, to examine the different frameworks and select the one that best matches your app’s needs.

