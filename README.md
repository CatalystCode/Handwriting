m# Handwriting
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

![alt text](https://github.com/CatalystCode/Handwriting/blob/master/Data/images/model_results.png)
