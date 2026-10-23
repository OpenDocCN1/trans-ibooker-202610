What happened here? This time, market demand was definitely strong: everyone on earth wants to see cancer eradicated. The first issue we see with this case is an unjustified focus on long-term AI vision. “Eradicating cancer” is as desirable as it is unlikely to be solved single-handedly by one tech company just because it closed a deal with a hospital.

On top of this, you'll be able to spot a whole series of technical mistakes as we shed some light on the inner workings of IBM Watson for Oncology. The medical technology was built on top of what IBM had developed to play the US game show *Jeopardy* and attracted some attention after winning against two human champions in 2011.

*Jeopardy* is a quiz show in which participants receive answers that they need to guess the questions for (basically, a reverse quiz). How did IBM transform the Jeopardy model to help oncologists? It started by finding suitable datasets. One of them was a set of 5,000 medical questions from the American College of Physicians (ACP). The following are examples of question/answer pairs in the dataset:

> *Q: The colorectal cancer screening test associated with highest patient adherence*
> 
> *A: Fecal immunochemical testing*

> *Q: Non-Hodgkin lymphoma characterized by extranodal involvement and overexpression of cyclin D1*
> 
> *A: Mantle cell lymphoma*

> *Q: Mechanism of acute varicocele associated with renal carcinoma*
> 
> *A: Obstruction of the testicular vein*

Basically, IBM Watson was using text data as input and output of the model. Since you learned in chapter 4 that text data is still the trickiest sector for AI, let's see what specific issues IBM had to face when transferring its technology into hospitals. At MD Anderson, both “acute lymphoblastic leukemia,” a kind of blood cell cancer, and “allergy” were often referred to with the acronym “ALL.” Obviously, cancer and allergies are two very different medical conditions, but Watson couldn't distinguish between the two because they used the same acronym.

Another issue is that Watson struggled to consider multiple aspects of a patient, leading to potential disasters. For example, it suggested that a 65-year-old man with diagnosed lung cancer and evidence of severe bleeding should be treated with a combination of chemotherapy and a drug called bevacizumab, which can cause “severe or fatal hemorrhage” and shouldn't be administered to patients experiencing serious bleeding.

What can we learn from this? We already knew some of the things that emerge from this article: text data is hard to deal with, especially when you're working in an extremely complicated context. In the width-depth framework we introduced to describe the complexity of an AI application (chapter 4), Watson would score extremely high in both width and depth. Width is high because the model needs to understand a vast spectrum of words coming from different branches of the medical field, and all the potential acronyms and abbreviations as well. Depth is also extremely