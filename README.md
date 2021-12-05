# Recognic Document Extraction with YOLO

### Functional Overview:
- Takes input from recognic-hub 
- YOLO extraction code only supports images, but as the image is a pre processed output from recognic-hub, that is already considered
- Provides response in .json format providing the prediction image url, response time, columns extracted and type of bank statement

### ML Overview:
Developed by Joseph Redmon and Ali Farhadi, YOLO(You Only Look Once)  is a real-time object detection algorithm that identifies specific objects in videos, live feeds, or images. YOLO uses features learned by a deep convolutional neural network to detect an object.
Documentation: [YOLO-documentation]


### Model Training and Testing:
YOLO model requires the setting up of certain files:
1. Config file containing the information about the Neural Network hyper-parameters
2. Weights file containing the weights used in the model
3. The object file having the name of the fields that have to be extracted

The specific step by step methodology of training and testing of YOLO model for custom data is available at: [How to train YOLOv3]. In our case our model is integrated into Recognic-Hub. So once the required files have been generated the final UT and QA are done through the Hub itself.
The flow can be better understood using the image shown below:

<img width="685" alt="Screenshot 2021-12-05 at 2 19 14 PM" src="https://user-images.githubusercontent.com/63894872/144740035-fe5befec-baa3-43bd-9bb4-d5da609a01b2.png">

### Hosting on Local Server:
To run the extraction code on a local server two local servers have to be hosted.
1. The Recognic-Hub server
2. The YOLO Extraction code server

#### Recognic-Hub Server
The following steps needs are followed in order to host the local server for Recognic-Hub:
##### 1. Clone the Git-Hub repository:
```sh
git clone --recurse-submodules https://gitlab.com/recognic/recognic-hub.git
```
##### 2. Install required libraries:
The following libraries have to be installed as required by the author for the proper function of the code
```sh
pip install -r requirements.txt
```
Some Mac users may face an issue with the ghostscript, in which case it would be advisable to not install ghostscript through the `requirements.txt` file and follow the below mentioned steps:
For newer user who are using M1 mac, ghostscript might show a missing libgs file error and the file would be unavailable at `usr/local/lib` 

The issue can be resolved by following these steps, in the same order:

- `brew install ghostscript`
- `conda install ghostscript`, which installs the arm_64 based library from conda-forge
- If conda throws a channel error try using `conda install -c conda-forge ghostscript`
- `pip install ghostscript`

##### 3. Hosting Server
- The local server of recognic-hub is hosted by running `manage.py`
- From the terminal one can get the IP where the server is hosted
- The remainder of the address for the api call can be found in the `url.py` code in the `adminapp` folder
- In our case we have the kyc_docs so the remainder of the IP right now is `api/v1/kyc/`

```sh
python manage.py
```
The api endpoint has a classifier which classifies the document into its category and passes on this information when calling the extraction code api endpoint

#### YOLO Extraction Code Server

Before the api call is made from the hub the local server for the extraction code should be up and running. The setting up of the server in this case has the following steps:
##### 1. Clone git-hub repository:
```sh
git clone https://gitlab.com/recognic/recognic-model.git
```
##### 2. Installing libraries:
Required libraries can be installed using a similar methodology which is:
```sh
pip install -r requirements.txt
```
##### 3. Hosting server
The local server is hosted by running the `flask_darknet.py` code
```sh
python flask_darknet.py
```
No further steps are required, as the api call is made from the hub

### Calling from Postman:
The api call can be made from post man. The required inputs in form-data tab of postman are:
1. key(file), file-type: image or pdf

The json response from the hub can be viewed in the terminal itself and the results can be verified. 


[//]: #
   [YOLO-documentation]: <https://opencv-tutorial.readthedocs.io/en/latest/yolo/yolo.html>
   [How to train YOLOv3]: <https://thebinarynotes.com/how-to-train-yolov3-custom-dataset/>
