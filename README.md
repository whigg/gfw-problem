# Global vessel detection system

Tracking vessel activity in the global oceans from space with AI and cloud computing

[Eye catching figure here]

## Summary 

This project describes the development of a state-of-the-art automated system for tracking, classifying and reporting vessel activities worldwide. The objective is two-fold: improve vessel detection accuracy and workflow efficiency. The approach leverages freely-available satellite radar and optical imagery, and state-of-the-art AI algorithms and cloud computing for global-scale monitoring of the oceans. We propose a two-step development process: First, implement an artificial neural network framework for ship detection using freely available Synthetic Aperture Radar (SAR) amplitude data that can be scaled globally. Second, with a fully working object-detection system in place, extend the data capability and model sophistication to improve detection accuracy, assimilating SAR polarization data and optical imagery.

## Motivation 

Illegal and unsustainable fishing practices can deplete marine resources and endanger food security. Illegal, unreported and unregulated (IUU) fishing, which is most prominent in countries with weak fisheries management and lax law enforcement, affects legitimate commercial fishers, impacts the accuracy of stock estimates, and induce severe damage to non-target species and vulnerable marine ecosystems. It is estimated that IUU fishing impacts the global economy on the billion-dollar-scale annually. Most developing countries do not have sufficient infrastructure in place to monitor vessel activity at large scale. And current global approaches rely on [describe deficiencies of CFAR]. There is a need for assessing activities 

## Improve detection

[AI, polarimetry, optical,...]

In recent years, computer vision methods have dominated the analyses of (natural, medical and satellite) images. In particular, deep learning approaches have been shown to outperform standard statistical methods on complex analyses such as classification, object detection and semantic segmentation, on near-real time massive data streams from surveillance systems, mobile devices, and commercial satellites. Unlike more traditional statistical methods that are data-type specific, the same CNN architectures have been implemented on different image types and complex backgrounds. CNNs are, therefore, a natural way forward to improve upon and extend the capabilities of current ship detection systems (moving beyond CFAR-based methods).

We propose to implement and test three CNN object detection methods: YOLOv3, Faster R-CNN, and SSD, as some studies suggest these are among the best performing CNNs for the task of ship detection on satellite iamges (see refs below). Python implementations of these CNNs on top of TensorFlow and/or PyTorch are also available. Despite the different strategies adopted by these three CNN architectures to locate objects, the output of the analyses consist on the center coordinates, bounding box with and height, and class probability of the objects, which is a convenient way to report location, size and uncertainty of a detected vessel.

Although the use of all-day/all-weather SAR backscatter information (as used by CFAR systems) constitutes a significant improvement over traditional optical methods (suffering from cloud coverage and light conditions), SAR images still suffer from inherent speckle noise and low contrast on rough ocean. Moreover, in-shore ships can be confused with the infrastructure of harbors, with similar brightness and shapes.

To address this limitation we propose to test the use of additional information (as a secondary step), such as SAR polarization and co-located detections from optical imagery.  

![PolSAR](images/sentinel-1-vv-vh.png)


## Improve efficiency

[Google compute engine instead of GEE, Parallel proc with Ray, optimized data formats, explore network structure simplifications (cite example), move full development to the cloud]


![Pipeline](images/pipeline.png)

**NOTE** I will not attempt to use the SAR phase information in the first implementation of the system. This is experimental and will likely require substantial research. This will also require additional development on the data engineering side: (a) data is not easily available and (b) the complex information will need to be pre-processed. I would first implement a DL framework to analyze Amplitude, then think how to incorporate Polarization and Optical information, and then (if we decide it’s worth pursuing based on small-scale tests) investigate incorporating Phase information.

Make clear this proposal is of practical character. We do not intent to develop new machine learning methods, but instead we aim to implement, test and adapt working methods and investigate optimal practices for the problem in question.

## Technical Overview

Regarding the choice of technologies and strategies adopted to develop the vessel detection system, there are a few practical considerations that must be taken into account. Overall, the framework needs to be:

- fast (aiming at near-real time detections in future)
- transparent (to facilitate implementation and modifications)
- scalable (identify and asses scalability bottlenecks early on)
- automated (or with as minimal human intervention as possible)
- proven (has been successfully applied, and showed potential to outperform CFAR)
- documented (accessible by any team member)
- open (based on actively maintained open-source code and publicly-available data)

Next we provide a sketch of the proposed development steps depicting the structure and rationale of the project. 

**[Be more specific (data sources, PolSAR, technical pre-processing, libraries/tools, NN arch]**

**[Also consider figures/visuals to make the points]**

**Architect pipeline**
* Identify data sources
* Identify data formats (original->optimal pipeline input->intermediate pipeline steps->output)
* Identify ingestion mechanism (cloud1->cloud2, external->cloud2)
* Identify cloud parallel framework (CPU and GPU)
* Architect cloud workflow: source1 + sourceN -> transformations -> cloud storage -> cloud development env (with access to storage and compute) -> cloud testing env (benchmarking/visualization) -> cloud deployment space -> cloud output
* Identify automation of pipeline steps (likely some human intervention will be needed at some points. What are those?)
* Make shareable/editable documentation

**Develop proof of concept**
* Implement a simplified/reduced version of the above pipeline
* Parallel framework: example, Ray for data pre-proc and ML preparation
* Select a couple DL approaches. Likely candidates: YOLOv3, Faster R-CNN (say why?)
* select a few (manageable) locations with identified data availability
* Device data labeling (manual vs semi-automated?)
* Only one object class at first: ship 
* Figure out optimal data transformation (e.g. filtering, cropping)
* Figure out best data augmentation approach (key aspect, large effort)
* Figure out representative training/testing data sets (what features need to be in the train/test data for best results? This is mostly unknown for remote sensing)
* Every DL implementation needs a baseline! We have the CFAR method :)
* Develop web-based visualization for intermediate and final products
* Make shareable/editable documentation

**Implement upscaled version**
* Think about challenges in this section [will need a lot of engineering]
* Perform global analyses
* Publish paper

**Improve implemented system**
* Data augmentation: how?
* Data combination (optical + radar)
* Extended object classes (containers, navy, cargo, passenger, fishing vessels, etc)
* Incorporate historic information to delineate strategic areas (e.g. protected ecosystems)
* Automated warning system? E.g. send message when vessel type crosses pre-defined boundary: intersection of polygon map with vessel location map
* Investigate how iceberg tracking methods apply to our problem
* Investigate how to merge optical imagery
* Combine results from CFAR and NN (evaluate differences)

How to test/validate system
Use AIS data

## Challenges

Given the adoption of novel technologies and global scope of the project, significant challenges still remain. As the project develops, we will investigate and update our adopted strategies. Some identified challenges are:

* Main challenges: 
	- Perform analysis at global scale (how to automate the full pipeline for global coverage and how to assess model performance/reliability of results at global scale)
	- How to generate optimal training SAR data set (quality and quantity)
* Other challenges:
	- Speckle noise (how to pre-process SAR for optimal training)
	- Sea state challenge (rough vs smooth)
	- Coastal challenge (multiple ship-like objects)
	- Cluster challenge (stack of ships: marinas)

[in narrative summarize challenges from papers] data, infrastructure, global validation

- most DL detection methods are for RGB images
- pre-trained models on RS images
- limited labeled RS data for training
- problems inherent to SAR (e.g. speckle noise, contrast on rough ocean)
- training DL models is computer intensive
- sea clutter in low and medium sea conditions (SAR)
the lack of detailed information about ships in SAR images results in difficulties for object-wise detection methods


## Final thoughts

What if it doesn't work? There is no guarantee that a Deep Learning approach will outperform a working method. The achievement of an optimal DL model for a specific problem relies on numerous trial-and-error tests (i.e. brute force), where the model is tuned for the specific data in question. Success heavily relies on a combination of creativity and domain expertise. If potential for outperforming the current approach is not evident at the initial stages (after substantial investigation), an alternative approach should be considered. For example, improving the current CFAR method with more traditional ML algorithms for pre- and post-processing SAR data and the inclusion of auxiliary information.

The file [example.ipynb](example.ipynb) is a Jupyter Notebook with a simple exercise to test setting up a basic CNN on a cloud GPU instance.

## References

YOLOv3 (C implementation) - https://pjreddie.com/darknet/yolo/

Faster R-CNN (Python implementation) - https://github.com/jwyang/faster-rcnn.pytorch

Ship-detection Planet data - https://medium.com/intel-software-innovators/ship-detection-in-satellite-images-from-scratch-849ccfcc3072

PolSAR for ship detection - X. Cui, S. Chen and Y. Su, "Ship Detection in Polarimetric Sar Image Based on Similarity Test," IGARSS 2019 - 2019 IEEE International Geoscience and Remote Sensing Symposium, Yokohama, Japan, 2019, pp. 1296-1299.

YOLO, Faster R-CNN, SSD - https://cv-tricks.com/object-detection/faster-r-cnn-yolo-ssd/
