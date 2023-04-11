# poisoned_output_detection

Here is my immediate theory of constructing a blackbox methodology to detect poisoned outputs of a given model (which can arbitrarily large). One would want to develope a black box methodology, owing to the fact that deployed models can go as big as 175 billion parameters. The main hypothesis of this apporach is that poisoned outputs are not prone to robustness, therefore exercise the robustness may reveal their identity:

![image](https://user-images.githubusercontent.com/47445756/231297814-a217fca0-71af-498c-8990-29d725f10ff5.png)


For a given model, that takes an input S (can be image or text) and outputs G (can be image, text or probability vector) .The methodology would be to add three different small-scale models (as shown in below figure) over this given original model:

The augmentor model augments the input data (possibly image), so that the image information is same but it is not same element wise This augmented original input $S_1$ is fed into the LLM to produce grammatically correct sentence again. Let’s call it $G_1$  . In this way we keep of creating augmenting random inputs into original sentences to form a aggregate set ${S_i }_{i>0}$. The grammatically correct sentence is ${G_i }_{i>0}$.
The similarity metric model compares the similarity of $G_i$ with G to compute similarity score $τ_i$. In this way the similarity profile is create as a time series. The similarity metric model can be as simple as a cosine similarity (if the output of original model is probability vector) or embedding of pretrained models (in case of image or textual output). Then the mean and standard deviation of the set $\{{τ_i}\}_{i>0}$ is calculated. If the mean and standard deviation are below certain thresholds, then the G does not contains poisoned parts, otherwise it contains poisoned parts.


In my demo of study like approach, I consider CIFAR-10 trained model as original model, and tried to reproduce the above study apporach by making the augmentor model approximate image and center cropped image. Making the similarity model as a simple cosine similarity function. And i didnt made the final classifier model, because it is my study, not a paper.

The main_model.ipynb point out towards poisoned model create and calculating the similarity profile. The "better_pure_model.ipynb" referes to create a non-poisoned model. The "augmentor.ipynb" creates the augmentor model, that fundamentally excerises robustness by generating querries similar to original querry to original model. Following is a sample input-output comparison of this augmentor model.

![image](https://user-images.githubusercontent.com/47445756/231302136-8c53ba91-81e5-4e5d-a1f5-8f58df8dc4f7.png)


The comaprison of similarity profiles of poisoned and non-poisoned model can be shown below.

![image](https://user-images.githubusercontent.com/47445756/231301494-8f356b8f-048b-4f97-9885-8883c61e0dd1.png)


It is clear that there is difference of temporal differences between the time series associated to difference categories, which can be classified by a sequence classification model.





