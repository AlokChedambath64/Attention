# Attention
 AI to predict a masked word in a text sequence.

# Background
One way to create language models is to build a Masked Language Model, where a language model is trained to predict a “masked” word that is missing from a sequence of text. BERT is a transformer-based language model developed by Google, and it was trained with this approach: the language model was trained to predict a masked word based on the surrounding context words.

BERT uses a transformer architecture and therefore uses an attention mechanism for understanding language. In the base BERT model, the transformer uses 12 layers, where each layer has 12 self-attention heads, for a total of 144 self-attention heads.

This project will involve two parts:

First, we’ll use the transformers Python library, developed by AI software company Hugging Face, to write a program that uses BERT to predict masked words. The program will also generate diagrams visualizing attention scores, with one diagram generated for each of the 144 attention heads.

Second, we’ll analyze the diagrams generated by our program to try to understand what BERT’s attention heads might be paying attention to as it attempts to understand our natural language.

# Understanding
First, take a look at the mask.py program. In the main function, the user is first prompted for some text as input. The text input should contain a mask token [MASK] representing the word that our language model should try to predict. The function then uses an AutoTokenizer to take the input and split it into tokens.

In the BERT model, each distinct token has its own ID number. One ID, given by tokenizer.mask_token_id, corresponds to the [MASK] token. Most other tokens represent words, with some exceptions. The [CLS] token always appears at the beginning of a text sequence. The [SEP] token appears at the end of a text sequence and is used to separate sequences from each other. Sometimes a single word is split into multiple tokens: for example, BERT treats the word “intelligently” as two tokens: intelligent and ##ly.

Next, we use an instance of [TFBertForMaskedLM]https://huggingface.co/docs/transformers/v4.31.0/en/model_doc/bert#transformers.TFBertForMaskedLM to predict a masked token using the BERT language model. The input tokens (inputs) are passed into the model, and then we look for the top K output tokens. The original sequence is printed with the mask token replaced by each of the predicted output tokens.

Finally, the program calls the visualize_attentions function, which should generate diagrams of the attention values for the input sequence for each of BERT’s attention heads.

Most of the code has been written for you, but the implementations of get_mask_token_index, get_color_for_attention_score, and visualize_attentions are left up to you!

Once you’ve completed those three functions, the mask.py program will generate attention diagrams. These diagrams can give us some insight into what BERT has learned to pay attention to when trying to make sense of language. For example, below is the attention diagram for Layer 3, Head 10 when processing the sentence “Then I picked up a [MASK] from the table.”

![image](https://github.com/AlokChedambath64/Attention/assets/110228030/4fe34a9f-dc9c-482f-a15b-b8a811a88237)

Lighter colors represent higher attention weight and darker colors represent lower attention weight. In this case, this attention head appears to have learned a very clear pattern: each word is paying attention to the word that immediately follows it. The word “then”, for example, is represented by the second row of the diagram, and in that row the brightest cell is the cell corresponding to the “i” column, suggesting that the word “then” is attending strongly to the word “i”. The same holds true for the other tokens in the sentence.

You can try running mask.py on other sentences to verify that Layer 3, Head 10 continues to follow this pattern. And it makes sense intuitively that BERT might learn to identify this pattern: understanding a word in a sequence of text often depends on knowing what word comes next, so having an attention head (or multiple) dedicated to paying attention to what word comes next could be useful.

This attention head is particularly clear, but often attention heads will be noisier and might require some more interpretation to guess what BERT may be paying attention to.

Say, for example, we were curious to know if BERT pays attention to the role of adverbs. We can give the model a sentence like “The turtle moved slowly across the [MASK].” and then look at the resulting attention heads to see if the language model seems to notice that “slowly” is an adverb modifying the word “moved”. Looking at the resulting attention diagrams, one that might catch your eye is Layer 4, Head 11.

![image](https://github.com/AlokChedambath64/Attention/assets/110228030/5b8caae2-4236-4795-bb93-c86014b7648e)

This attention head is definitely noisier: it’s not immediately obvious exactly what this attention head is doing. But notice that, for the adverb “slowly”, it attends most to the verb it modifies: “moved”. The same is true if we swap the order of verb and adverb.

![image](https://github.com/AlokChedambath64/Attention/assets/110228030/c561f7b7-c4df-4857-ac5b-d74ebd43d551)

And it even appears to be true for a sentence where the adverb and the verb it modifies aren’t directly next to each other.

![image](https://github.com/AlokChedambath64/Attention/assets/110228030/be97bd9d-ef42-42f3-aa87-52f2cb0dca33)




