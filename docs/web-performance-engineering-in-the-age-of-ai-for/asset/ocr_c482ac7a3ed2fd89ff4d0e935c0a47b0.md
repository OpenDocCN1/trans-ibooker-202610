# Accessibility issues

Accessibility is often an afterthought in much of the code found in public repositories, and LLMs reflect that gap. Empirical evaluations have highlighted that AI-generated UIs tend to have significant accessibility violations. In one 2024 study, 88 developers prompted ChatGPT to build websites and then checked them with automated accessibility tools. The results showed that a large majority (84%) of ChatGPT-generated websites contained multiple accessibility problems.

Common issues included missing alternative text on images, lack of semantic HTML structure for screen readers, insufficient color contrast, and missing keyboard navigation support. The study’s authors had to iterate with increasingly specific prompts to fix these problems—something only a knowledgeable developer would know to do. The clear takeaway was that you cannot rely on a chatbot to produce production-ready accessible code. An AI may, for instance, create a visually appealing button but forget the ARIA label needed for screen readers, or lay out content in a way that makes sense visually but is incoherent when linearized for a blind user. These oversights can alienate a segment of your users and even pose legal risks in regions with strict accessibility laws.

# Poor UX and polish

Beyond raw metrics and accessibility compliance, there are myriad subtle UX aspects that AIs often overlook. For example, an AI-generated form might not include client-side validation or helpful error messages, frustrating users. Or it might not implement responsive design properly, causing a layout to break on mobile devices.

Another example is the lack of performance-oriented UX considerations: an AI might fetch a large dataset and render it all at once in a single list, without any loading spinners or progressive loading. Users facing a blank screen might think the app is frozen, whereas a human developer would likely implement a loading indicator or