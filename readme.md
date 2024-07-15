Here is the description of the solution

## LLM
The main idea is to import and inference one of the state-of-the-art llm models.

For this solution microsoft/phi-2 (https://huggingface.co/microsoft/phi-2) is used

## Instagram data
We can specify desired style of our post by providing examples(from the scrapped instagram data) to the llm in a prompt 

After reading the file with scrapped data and vectorizing it, we can find the most similar post to our topic using faiss.IndexFlatL2 and .search (fn. get_relevant_texts())

## Prompt
phi-2 works really bad if we exceed the optimal length of the prompt. In order to adress this problem, the main inference function will autogenerate prompt from the "main_topic", "model_name"(backpack model) and "features" parameters

The prompt itself:
"Instruct: Imagine you are a online company selling sport backpacks. Write a short instagram specific post about {main_topic}. Mention only {features} and name of the product {model_name}. Promote activities associated with this product and stick to this style: {relevant_texts} \nOutput:"
Where relevant_texts - the closest instagram posts

## Results
main = "backpack"
model_name = 'ADV5'
features = 'camping, hiking, 200 liters'
relevant_texts = True

Output:
🌲🏞️Ready for your next adventure? Check out our new ADV5 backpack! 🎒🐶 Perfect for camping and hiking, this backpack can hold up to 200 liters of gear. Don't forget to pack essentials like a tent, sleeping bag, and cooking supplies. And of course, don't leave your furry friend behind - bring them along for the hike! 🐾 #hiking #camping #adventure #backpack #hikingchecklist #ospreypacks

LLM mentioned "furry friend" because the closest intagram post to our "features" mentions dog.
If we turn off relevant_texts = False:

Output:
🌲🏕️🥾 Calling all adventure seekers! 🌲🏕️🥾 Introducing the ADV5, the ultimate camping and hiking backpack with a whopping 200 liters of storage space. 🌲🏕️🥾 Whether you're exploring the great outdoors or embarking on a multi-day trek, the ADV5 has got you covered. 🌲🏕️🥾 With its durable construction and comfortable straps, you'll be ready for any adventure that comes your way. 🌲🏕️🥾 Don't miss out on this must-have gear for your next outdoor escapade. 🌲🏕️🥾

## Possible Improvements
The main disadvantage of phi-2 is relatively small prompts (150 tokens).

For the improvements of out solution we can get more computational power and use bigger models like llama-7b, which can hadle bigger prompts and produce better results