### Pipeline for TensorFlow Object Detection - by Vlad Negreanu

TensorFlow Object Detection Model Pipeline has three phases:

    - Annotating images and serializing the dataset
    - Choosing a neural network and preparing the training pipeline
    - Training the network
	
##### Annotating images and serializing the dataset

	1.  Install [labelImg](https://github.com/tzutalin/labelImg). This is a Python package, which means you can install it via pip, but the one from GitHub is better. It saves annotations in the PASCAL VOC mode in xml based format.
	2.  Annotate your dataset using labelImg.
	3.  Use [this script](https://github.com/datitran/raccoon_dataset/blob/master/xml_to_csv.py) to convert the XML files generated by labelImg into a single CSV file.
	4.  Create a "label map" for your classes. You can check [some examples](https://github.com/tensorflow/models/tree/master/research/object_detection/data)
    to understand what they look like. You can also generate one from your original CSV file with [this script](https://github.com/douglasrizzo/detection_util_scripts/blob/master/generate_pbtxt.py).
	5.  Use [this script](https://github.com/datitran/raccoon_dataset/blob/master/generate_tfrecord.py) - adjust `class_text_to_int` function according to your classes
    to convert the CSV file (`train.csv`) into a TFRecord file (eg. `train.record` ), a serialized
    data format that TensorFlow is most familiar with. You will need the label map from the previous for this.
	
##### Choosing a neural network and preparing the training pipeline

	7.  Download one of the neural network models provided in [this
	page](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md).
	The ones trained in the COCO dataset are the best ones, since they were also trained on objects.
	8.  Provide a training pipeline, which is a `config` file that usually
	comes in the `tar.gz` file downloaded in the previous step. If they don't come in the `tar.gz`, they can be found
	[here](https://github.com/tensorflow/models/tree/master/research/object_detection/samples/configs).
		- The pipeline config file has some fields that must be adjusted
    before training is started. Its header describes which ones.
        - Usually, they are the fields that point to the label map, the
    training and evaluation directories and the neural network
    checkpoint and the TFRecord.  
		- Other parameters to take care about are the number of steps, 
	number of examples and the number of classes.
	
##### Training the network

	9.  Train the model. [This is how you do it locally](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/running_locally.md).
    10. Export the network, like [this](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/exporting_models.md).
    11. Use the exported `.pb` in your object detector.