# Wheelchair_Detector

 Add short description of project here >

![add image descrition here](direct image link here)

## The Algorithm

Add an explanation of the algorithm and how it works. Make sure to include details about how the code works, what it depends on, and any other relevant info. Add images or other descriptions for your project here. 

## Running this project

### Part One: Connecting to your Jetson Nano
1. Open the Start menu on your Windows computer and type "Mobile hotspot"
2. Click on the System Settings link that appears
3. Ensure "Share my Internet connection with other devices" is enabled
4. Reopen the Start menu and type "Visual Studio Code"
5. Click on the Application link that appears
6. Click on the blue button in the bottom left-hand corner to open a Remote Window
7. In the text box that appears, select "Connect to Host..."
8. In the text box, type ```nvidia@xxx.xxx.xxx.xxx``` where the x's represent your Nano's IP address
9. Enter the Nano's password to confirm (this password is usually "nvidia")
10. Click on the page icon in the top left to open the Explorer and click "Open Folder"
12. In the text box that appears, type ```/home/nvidia/``` and press enter
13. Enter the password to confirm
14. You have successfully connected to your Jetson Nano!

### Part Two: Training the Model
1. Using the VS Code Terminal window, navigate to "jetson-inference/python/training/classification/data" (```cd jetson-inference/python/training/classification/data```)
2. Type ```wget <LINK HERE> -O wheelchair_detector.tar.gz``` to download the dataset
3. Type ```tar xvzf wheelchair_detector.tar.gz``` to unzip the dataset
4. Navigate to "jetson-inference" (```cd jetson-inference```)
5. Type ```./docker/run.sh``` to enter the docker
6. Navigate to "jetson-inference/python/training/classification" (```cd python/training/classification```)
7. Type ```python3 train.py --model-dir=models/wheelchair_dectector data/wheelchair_dectector``` to train the model. This can take a while! (optional arguments include: ```--batch-size=<8>```, ```--workers=<2>```, and ```--epochs=<35>```, where <#> represents default values)
8. Type ```python3 onnx_export.py --model-dir=models/wheelchair_detector``` to export your model
9. Press ```Ctrl + D``` to exit the docker

### Part Three: Running the Model
1. Navigate to "jetson-inference/python/training/classification"
2. Type ```NET=models/wheelchair_detector``` to set the NET variable
3. Type ```DATASET=data/wheelchair_detector``` to set the DATASET variable
4. Type ```imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt <input> <output>``` where <input> is the relative path of the photo to be analyzed and <output> is the relative path of the to-be exported photo

[View a video explanation here](video link)
