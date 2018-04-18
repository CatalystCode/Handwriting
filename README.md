# Handwriting
Code and procdures for handwriting object detection and recognition

# Business Problem
We teamed with major global professional services organization to improve their contract search and knowledge extraction results.  Their professional services personnel spend significant amounts of time reading and extracting information from their clients’ contracts in the course of their work. Accurate automated contract search for entities and concepts would significantly reduce the amount of time they need to spend on this routine work.  As they prepared their digitized legal agreements to be searched, they discovered handwritten annotations presented a major obstacle to effectively retrieving key information from this content.  Even today, Interpreting handwritten annotations is error prone in traditional methods of optical character recognition (OCR).   And these handwritten notes are critical to understand, as they often amend or define critical terms of the agreement terms.  

The handwriting recognition arises from varying degrees of legibility, handwriting style as well as the fact that handwritten annotations can appear in any location on the machine printed contract page.  Compounding these difficulties, the digitized version of these contracts is often in PDF format, scanned in final executed copies of agreements one or two generations removed from the original.   Given these realities, we chose to work together to enabling robust search of these contracts is to identify and recognize this handwriting and convert it to printed text. 

# Problem Statement

Optical character recognition (OCR) technology with standard tooling performs poorly at recognizing handwritten characters on a machine printed page.   Characters are recognized incorrectly, and OCR often misplaces the location of the handwritten information when melding it inline with the adjoining text.  And while pure handwriting recognizers have long had stand-alone applications, there are few solutions that work well with document OCR and search pipelines.  

Then once the handwriting on the printed page has been identified, we need to insert these recognized characters back into the text, placed into the correct location in the printed page.  And this needs to be a seamless part of the search preparation workflow.  To date, these handwriting recognition models and services have been poorly integrated into this workflow.  

In recent years computer vision object detection models using deep neural nets have proven effective at a wide variety of object recognition tasks.   And the large pre-trained image models, developed and trained with millions of labeled images can be used to improve these object detection via transfer learning, a method of retraining an existing pre-trained model to accomplish a different but related task.   Using pre-trained models via transfer learning dramatically reduces the number of labelled training data required to achieve the performance needed on any object detection task.  While object detection and transfer learning are being broadly applied to a wide variety of use cases, there are few applications to this very common business problem of locating handwriting on a page within the workflow of preparing digitized documents for entity extraction and search.

For this project, we needed to solve the end-to-end problem of detecting the text, interpreting the characters of the handwriting, then inserting the interpreted text back into the contract image in preparation for OCR and knowledge extraction.

# Approach

To start, we applied object detection to detect handwriting and identify its bounding box location on an image of a contract printed page.  For our problem, transfer learning from a pre-trained model was an obvious choice, given our small sample of labelled handwritten annotation and large pre-trained models with related information.  

We were able use new Azure ML Package for Computer Vision (AML-CVTK), with its prepared Jupyter notebook on object detection and customizable utilities and functions for data preparation and transfer learning.   The data preparation utilities made our work much faster.   The AML-CVTK notebook and supporting utilities take advantage of the Faster R-CNN object detection model, which has produced state-of-the-art results in object detection challenges in the field.  In the back-end, AML-CVTK uses Tensorflow's implementation of Faster R-CNN.  You can find more details on the customizable parameters available on the Tensorflow object detection website.  The utilities enable transfer learning on a selection of pre-trained models natively.  We selected the Coco Common Object in Context dataset (mobilenet_v1_coco_2017_11_17), a model with >200k labeled images, 1.5 million object instances across 171 categories, some related to our handwriting task, with recognition in context.   

For our data set, we manually labeled a small set of public government contract data with both machine printed text and handwriting using VOTT tool, which we detail in the data section below.  We labelled two classes of handwriting, a signature and a non-signature in the VOTT tool, recording the bounding box of the handwriting.  This relatively small set of labeled data were passed into the AML-CVTK notebook to train a handwriting detection model. 

Once we had recognized the handwritten annotations, we used the Cognitive Services Computer Vision API to recognize the characters of the handwriting. Our workflow, from object detection to handwriting recognition and replacement in the contract, is summarized in Figure 1 below.



