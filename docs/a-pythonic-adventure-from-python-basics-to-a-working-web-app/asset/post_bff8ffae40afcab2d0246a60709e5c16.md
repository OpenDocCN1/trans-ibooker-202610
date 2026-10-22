
+   - Free license More info
+   - Attribution is required How to attribute?
+   - File type: AI, EPS, JPG How to edit?

He clicked 'More info' and checked the license terms:

## License summary

Our license allows you to use the content

- For commercial and personal projects
- On digital or printed media
- For an unlimited number of times and without any time limits
- From anywhere in the world
- To make modifications and create derivative works

"Sounds good to me. We are allowed to use it, but we have to use attribution—see this Attribution is required phrase? Let’s see what we should do," and he clicked the How to attribute? link.

## How to attribute?

Creating content takes a lot of time and effort, which is why we ask you to provide a link to attribute the source. Where will you be using the image?

Web Printed items Video Apps and games

For example: websites, social media, blogs, ebooks, newsletters, etc.

Copy this link and paste it in a visible place, close to where you're using the image. If that's not possible, place it at the footer of your website, blog or newsletter, or in the credits section.

Image by Freepik

Copy

"We have to copy the text and add it on our page next to the picture. No problem," he said and clicked the blue Copy button.

"Now we have to add it to our order page," he said, opened the order.html file, and pasted the text at the bottom of the page, just before the {% endblock %} line. "We will move it later, when we place our picture," he explained.

"Now download the picture," he said to Erik. Erik clicked the green Download button, then the Free Download drop-down menu and selected JPG.

![](img/f6295b544c84568800930d6ec95963ae_204_0.png)

The image file name `1544839_222805-P1KRC2-559.jpg` appeared at the bottom of his browser.

**NOTE** In your case the file name could be different. Please use your file name in the following code changes.

Simon said, "Now copy this downloaded image file from the Downloads folder to your project folder. You have to copy it under `coffeeshop/static/img/`. Can you find it?"

"I think so," Erik said and opened the file manager program (Finder on his Macbook). After several mouse moves and clicks he found the right location. His project directory now looked like this:

"Great," Simon said. "Now we have to place it on the page. Open again the order.html file. Let me help you," and Simon added a line just before the one with the attribution he copied earlier.

The bottom of the order.html file now looked like this:

## Listing 13.9 templates/order.html: Add a picture

```
...
<img src="/static/img/1544839_222805-P1KRC2-559.jpg"
 alt="Coffee drinks" width="100%">
Image by <a href="https://www.freepik.com/free-vector/
colorful-coffee-cup-set-flat-style_1544839.htm">Freepik</a>

{% endblock %}
```

He saved the file and said to Emily and Erik: "Now reload the page and we’ll see if it worked."

Emily clicked Reload and they saw the updated page:

## Welcome to E&E Coffee Shop

ENTER NAME - Choose drink - - Choose flavor - - Choose topping - SUBMIT

![](img/f6295b544c84568800930d6ec95963ae_206_0.png)

Image by Freepik

## YOUR TURN Add a picture to your page

Find a picture you like and add it to your order page. Don’t forget to check the license and follow the instructions on attribution.

> "Great job!" Simon said. "It looks like a real online shop now. What’s important—it not only looks great but it works too!"

Emily and Erik were happy to see the result of their hard work. It wasn’t easy to gather all the small pieces together but the result was overwhelming—it was a real web application that they wrote themselves.

> "I have an idea," Simon said. "You know what AI, or Artificial Intelligence is, right?"

> "Yes, all those silly chatbots and programs that create funny pictures," Erik answered. "We tried some of them with friends."

"Yes, but I'm talking about AI that can help us, developers," Simon said. "Some tools can help us write functions, improve our code, explain error messages."

"Yes, I saw some demos," Emily said. "They wrote whole working programs. I looked like magic. Simon, does that mean AI can write an application like the one we just wrote?"

"Probably," Simon said. "But still, it can't do it without human help. First, you have to come up with an application idea. Like Erik did several weeks ago. Second, you have to explain the the AI what you want from it. You have to be very specific, otherwise the AI will do something that you wouldn't expect. Third, you will have to review the code and make sure it works as you want it to work. And, of course, no AI can choose the website colors, fonts, and pictures that you like. Like you just did with Erik. It's a matter of taste and no AI can do it, as far as I know."

"Yes, AI can be a great help," he continued, "but so far it can't completely replace human developers and designers. Let's try it tomorrow and see how AI can help us."

## New things you have learned today

- **Different kinds of fonts**
  Serif, Sans Serif, Monospace are different types of fonts that you can use in your application. Serif is good for headers, book and documentation texts. Sans Serif is used for menus, but also for headers and text. Monospace is used to demonstrate program code, file names, and command examples.
- **Color codes**
  Colors can be described using *hexadecimal* numbers, like #a1d4c5. They consist of three base colors: red, green, and blue. You can specify colors for your web page using these color codes: for background, menus, headers, text, etc.
- **Pictures have authors and licenses**
  It's important to know that not all pictures you can find on the Internet you can use freely on your web page. Pictures and photos have authors and they decide how you can use their work. Very often you can use their pictures without paying them, but you should place an *attribution* on your page. It's a way to say "Thank you" for their work.

## Code for this chapter

You can find the code for this chapter here:
https://github.com/pavelanni/pythonicadventure-code/tree/main/ch13

## Help from AI: Improving our code

## In this chapter

- Erik and Emily ask AI for help
- AI gives code reviews
- AI writes comments
- AI explains error messages
- AI explains itself
- AI gives other ideas on how to use AI

"Are you ready for getting help from AI?" Simon asked Erik and Emily.

"Sure," Erik answered. "Is there a special AI site for developers?"

"There are several AI tools for developers," Simon said. "With some of them, you log in to the site and ask for help. You ask for code review, or for code explanation, for example. Other tools can be installed in your editor and they help you write the code. You will try it later, so today let’s start with using a website. I have an account at ChatGPT—let’s use it."

Simon opened a web page at https://chat.openai.com, entered his login name and password, and they saw a simple page with a prompt "Send a message..." at the bottom.

"Remember how I told you yesterday," he started, "that you have to explain precisely to AI what you want from it? It’s not a chatbot where you can start with just 'How are you?' We want to get a code review for one of our functions so we have to explain it that way. It’s recommended that we tell the AI that it has to act as somebody. In this case we’ll ask it to act as a code reviewer. And then we’ll tell it what we expect from it."

Simon started typing in the prompt window:

```
Act as a code reviewer. Your task is to receive a function from me and
give me feedback on the code structure, syntax, any potential errors, and
improvements that can be made.
Here is my first function to review:
```

He stopped and said, "Let’s begin with a simple function. For example, let’s take the read_menu() function." And he copied the function from the editor:

```
def read_menu(filename):
    f = open(filename)
    temp = f.readlines()
    result = []
    for item in temp:
        new_item = item.strip()
        result.append(new_item)

    return result
```

The AI started typing almost immediately:
