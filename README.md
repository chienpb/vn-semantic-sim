# vn-semantic-sim

This is a notebook project that finetune large language model (Mistral 7B, PhoGPT 7B) and pretrained model (PhoBERT) to tell if whether two Vietnamese sentences have the same meaning (semantic similarity).

## PhoBERT
To calculate the similarity score of two sentences using sentence transformer model, we must get the two sentence embeddings $v_1$ and $v_2$. After that, we calculate the cosine similarity:
$$sim = \frac{v_1\cdot v_2}{|v1||v2|}$$

Then, we compare the cosine similarity with threshold $t$. The two sentences will be considered to convey the same meaning if $sim > t$. The F1-score of the base PhoBERT model on the vnPara dataset is 93.48%.

To finetune the model, I used the Vietnamese-translated STS-Benchmark dataset (Vi-STS). The dataset contains 3 columns, the first two columns are the raw sentence and the last column is the similarity score of them on the scale from 0 to 5 (with 0 being totally different and 5 being completely the same). After finetuning with 10 epochs, the F1-score increased to 95.84%

## Large Language Model
To be able to use the base model on free version of Kaggle, I quantized the model to 4-bit using bitsandbyte library. Then I do some prompt engineering to find the best prompt for the task. After the inference process, the result of base Mistral 7B and PhoGPT 7B model on vnPara dataset are 80.5% and 64.12%.

To finetune the model, I used qLoRa technique with 4-bit quantization. The F1-score increased to 84.4% and 64.37%.
