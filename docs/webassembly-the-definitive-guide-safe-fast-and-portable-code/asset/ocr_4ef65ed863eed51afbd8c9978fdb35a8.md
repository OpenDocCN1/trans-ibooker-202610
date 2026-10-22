The demo app is quite sophisticated and overwhelming as a means of getting started, but to show you a bit about what the code looks like, please see Example 10-17 taken from the GitHub documentation.

# Example 10-17. Code snippet for building a simple application

```
ui.heading("My egui Application");
ui.horizontal(|ui| {
    ui.label("Your name: ");
    ui.text_edit_singleline(&mut name);
});
ui.add(egui::Slider::new(&mut age, 0..=120).text("age"));
if ui.button("Click each year").clicked() {
    age += 1;
}
ui.label(format!("Hello '{}', age {}", name, age));
```

The results of this code sample are shown in Figure 10-6.

![](bbox=[0.145, 0.425, 0.855, 0.63])

Figure 10-6. Rendered form of the sample code

If you are interested in building your own application using egui that will work either natively or in the browser, Emil has provided a working project template at https://github.com/emilk/egui_template. Just fork the repo (or use the GitHub template to create your own repo) and follow the instructions to get up and running.

I hope you have fun playing around with this cool library and its related infrastructure. My larger point is that it represents the intentional design of software to target both native and browser-based application delivery. I expect to see much more of this approach before long.