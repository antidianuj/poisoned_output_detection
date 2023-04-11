# poisoned_output_detection

Here is my immediate theory of constructing a blackbox methodology to detect poisoned outputs of a given model (which can arbitrarily large). One would want to develope a black box methodology, owing to the fact that deployed models can go as big as 175 billion parameters.

![image](https://user-images.githubusercontent.com/47445756/231297814-a217fca0-71af-498c-8990-29d725f10ff5.png)


For a given model, that takes an input S (can be image or text) and outputs G (can be image, text or probability vector) .The methodology would be to add three different small-scale models (as shown in below figure) over this given original model:

The augmentor model augments the input data (possibly image), so that the image information is same but it is not same element wise This augmented original input $S_1$ is fed into the LLM to produce grammatically correct sentence again. Let’s call it $G_1$  . In this way we keep of creating augmenting random inputs into original sentences to form a aggregate set ${S_i }_{i>0}$. The grammatically correct sentence is ${G_i }_{i>0}$.
The similarity metric model compares the similarity of $G_i with G to compute similarity score $τ_i$. In this way the similarity profile is create as a time series. The similarity metric model can be as simple as a cosine similarity (if the output of original model is probability vector) or embedding of pretrained models (in case of image or textual output). Then the mean and standard deviation of the set ${τ_i }_{i>0}$ is calculated. If the mean and standard deviation are below certain thresholds, then the G does not contains poisoned parts, otherwise it contains poisoned parts.




