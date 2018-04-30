# Handwriting
Code and procdures for handwriting object detection and recognition

# Business Problem
We teamed with major global professional services organization to improve their contract search and knowledge extraction results. 

# The Solution
- We used handwriting object detection followed by OCR on the non-signature handwriting.  
- We inserted the OCR handwriting into the rest of the machine printed OCR result.
- We used the new Azure ML Package for Computer Vision to make the object detection model solution simpler.

# The Data
- We used a public repository of government contracts available here. https://www.gsa.gov/real-estate/real-estate-services/leasing-policy-procedures/lease-documents 
- We used the VOTT data labeling tool to speed our data labeling task, available at this link. https://github.com/Microsoft/VoTT
- We have a sample of our labeled data in the data folder in this repo.  
- We have the full sample of our labeled data available at this URI. https://handwriting.blob.core.windows.net/leasedata.   

![alt text](https://github.com/CatalystCode/Handwriting/blob/master/Data/images/sample_contract.png)

# The Results
For each image, we defined two groups:
-	At = union of pixels inside true label bounding boxes (ground truth, green squares below). 
-	Am = union of pixels inside bounding boxes found by the model (red squares below). 

Then we used the union of these two groups, instead of the sum, to account for overlapping boxes. We defined ‘success’ for our objective as: 
-	Ai = intersection between At and Am. 

-We refined the traditional intersection over union (IOU) object detection results measures for our task, as we were interested in our ability to retrieve just the areas of interest within a machine printed text, and within the non-signature handwriting to detect as precisely as possible to enable accurate OCR in the later process.  We defined our results on a per image (or per page) basis as follows:
-	per-image recall = Ai / At, i.e. the fraction of target pixels actually covered by the model.  
-	per-image precision = Ai / Am, i.e. what fraction of the pixels detected were in the actual handwriting box. 

-Further, we measured 71 contract pages from our test set which had both signature and non-signature handwriting present on the image from the same government contract data source, not yet seen by our model.  We calculated per image recall and precision for each category with our test set.  We also plotted the boxplot in Figure 6 showing min, max, 25% quantile, 75% quantile and median of these metrics over all the test images.  

- Our results on the handwriting object detection were relatively good for both signature handwriting detection and non-signature handwriting object classes.  The performance of non-signature handwriting detection is slightly worse and more variable than that of signature handwriting detection. 

- For one contract page, if the model detects non-signature handwriting but there is no non-signature handwriting in ground truth in that detected location or vice versa, we will define precision = 0 and recall =0, which gives us a conservative performance measure. 

- Figure 7 gives an example where there is a missing labeling in the ground truth, but the model detects it. On the figure, the green box represents the ground truth and the red box is model prediction. In this case, we defined that precision and recall=0. Figure 6 shows that over 25% percentile of the data for non-signature precision and recall are zero.  Manual inspection shows that some of these 25% represent incorrect labeling or noisy artifacts of the scan being recognized incorrectly.  Potentially additional training data could improve these results.

![alt text](https://github.com/CatalystCode/Handwriting/blob/master/Data/images/model_results.png)


# Conclusion
- We invite your feedback and contributions to this solution!
