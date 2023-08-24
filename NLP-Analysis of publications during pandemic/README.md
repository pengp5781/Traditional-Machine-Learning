# Overview
The project served to learn useful insights from 400000+ journals regarding infectious disease since 2001. 
The main purpose was to discover more about the influence of COVID-19 pandemic on acedamic research and it was achieved by a series of preprocessing of string data and application of unsupervised machine learning methods.

# Methods
### Data investigation and preprocessing
The metadata contains everything from titles, abstracts, main essay to, author name, publication time, etc. 170000+ publications were selected by filtering publication time and keywords in title, abstract as only COVID-19 related articles were concerned with the purpose.

A word cloud overview of main keywords in the articles:

![image](https://github.com/pengp5781/Traditional-Machine-Learning/assets/111671117/81e6c5cb-d6ec-4e53-a59a-42d6be0dd551)

As shown above, discussion revolving around the pandemic was not only limited to how the disease can be cured. There are various topics like:
1) "artificial" "intelligence".
2) "SARS", here it might be comparing the virus to sars in hope to learn from the past.
3) "Backgound", "originating" indicates discussion about origination of the virus and so on.

It's also noticed that how fast-responding biomedical scientists are -- more than half the total publications were contributed in January 2020 right after the outbreak.

![image](https://github.com/pengp5781/Traditional-Machine-Learning/assets/111671117/49f81e8b-d10e-441a-b762-02fdb9387afb)

After the investigation, it was soon determined that a classification model could be created to organize the data into catogories to see how each aspect of the pandemic was investigated. To achieve this, I kept the title and abstract fields and applied a series of processing steps(removing punctuation, stopwords, special chars, etc) to clean the data.The reason why only the title and abstract contents were kept was due to them being the most relevant information for finding categories and the lack of computation power to analyze full-size data(project was made to be ran on google colab)

### Everything about the model
TF-IDF was used to represent the word data but for bigger dataset like this, having 10000+ features was not quite realistic as the model would run well over 40 minutes to fit. Dimensionality reduction was urgently needed but a standard PCA would explode memory with such sparse matrix. After some research, I found that Truncated SVD does not cause the same problem and is commonly used in the industry to address similar matter. Therefore, TF-IDF transforming was chained to truncated SVD to construct input matrix to machine learning model.

I used KMeans algorithm which is a classic but powerful clustering method and it takes in hyper-parameter "k" that indicates number of clusters. Since there are no target values to evaluate the performance of models, I used "Elbow method" to find the most optimal k value. Evaluation is based on the sum of squared distance from the points to the cluster they are assigned to. This sum of distance gets smaller with larger k value(i.e, more clusters) and it equals zero when k is set to number of observation. However, we don't want crazy many clusters, to find optimal k is to find the right amount of clusters that yields relatively low squared error. So I tested 20 models with k value ranging from 1 to 20 and plot their corresponding squared distance. The plot is showing a not so obvious arm-like curve and the turning point/elbow point on arm is determined as optimal point. In this case, optimal k is 9.

![image](https://github.com/pengp5781/Traditional-Machine-Learning/assets/111671117/ca96091a-d743-434c-bafc-13ca225f355f)


# Conclusion
### Results
- The data is classified into 9 groups. Group 2 is the biggest with 103886 papers labelled in. Group 8 has the least amount of observations that their wordcloud visualizations is meaningless.
  From wordcloud plotting, it seems like group 2 and 0 likely gathered papers that discuss around the properties of the virus and it's similarity to SARS. Words like syndrome, SARS, infection are mentioned frequently.

***Group 2 Wordcloud:***

![image](https://github.com/pengp5781/Traditional-Machine-Learning/assets/111671117/dd54ce85-52c2-4c51-87cf-93bb847d2199)

- About 2000+ papers are in Spanish as group 7 and 3 should be groups of Spanish papers since the Spanish key words in plot. This might suggest that only minority (about 1%) of scholarly articles are produced by Spanish speaking countries.

***Group 7 Wordcloud:***

![image](https://github.com/pengp5781/Traditional-Machine-Learning/assets/111671117/06eae124-02a0-43de-b930-84e454c529ee)

- For most categories, there are 2 peak time:January 2020 and January 2021 for paper publication. Only papers in group 3 are published in another peak time at September 2020.

***Group 3 numebr of publication bar chart:***

![image](https://github.com/pengp5781/Traditional-Machine-Learning/assets/111671117/939d5c81-a51d-4b6e-8db8-6a69232aca27)


### Insights

From a government perspective, it’s good to see how much effort scholars  are contributing to tackle the COVID-19 pandemic in a basic level (i.e to cure the disease). However,  with all the attention to disease itself and to already infected patients, not much of meaningful discussion are about the means to prevent it from spreading at the first place. Preventing is just as important as curing but the methods government are adapting now (i.e mask wearing, lockdown) do not seem to be all that effective. The governments need more guidelines so they might want to push academic world to look into the mechanism behind all the prevention methods and try to improve them or even build up new protocols that are more practical.

From a healthcare system point of view, there had already been tremendously much medical force devoted to the caring of existing patients. Now that with us being more informed about the disease and less patients in intensive care unit, the system probably want to consider allocating more forces to deal with the aftermath of the pandemic. For example, mental issues seem to be a big problem that are studied quite often together with the COVID-19 matters. Caring for those who aren’t infected but have mental illness caused by either fearing the disease or long-term lockdown would be top priority to prepare for when the pandemic enters next stage.

From a scholar point of view, since properties of disease itself and the curing/vaccine for it are already well-studied, one might want to consider researching into COVID-19 from angles other than epidemiology or virology.  As mentioned above, learning psychological problem caused by the pandemic can be a good direction. Also one can learn about why prevention is perfectly done in some countries but not working so well in others(i.e learning reasons behind refusal to wearing mask, obligation of lockdown order and etc ).


