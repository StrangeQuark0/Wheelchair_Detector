# Wheelchair_Detector

This project uses a custom-trained AI model and Nvidia's Jetson Nano to identify wheelchairs in webcam footage and send out a ping when one is detected. This project is designed to be integrated with a motorized door so that approaching wheelchair users are spared the need to press a button or open the door manually. 

---

![Imgur](https://i.imgur.com/UIpCtVrm.jpg)
![Imgur](https://i.imgur.com/6qRImaom.jpg)

![Imgur](https://i.imgur.com/qlBgixUm.jpg)
![Imgur](https://i.imgur.com/GqNoL7Jm.jpg)

---

## The Algorithm

This AI detection model was created by retraining the ResNet-18 neural network and operates using a script modified from a program called ImageNet. ImageNet identifies the objects in photos based on a predefined classification system. This AI model replaces that framework with a set of user-defined categories and online image sets designed specifically to identify media containing wheelchairs. The model receives input from a webcam and logs each instance of a wheelchair to the terminal. When integrated with machinery, this log could be used to autonomously trigger a motorized door. 

## Running this project

### Part One: Connecting to your Jetson Nano
1. Open the Start menu on your Windows computer and type `Mobile hotspot`
2. Click on the System Settings link that appears
3. Ensure "Share my Internet connection with other devices" is enabled
4. Reopen the Start menu and type `Visual Studio Code`
5. Click on the Application link that appears
6. Click on the blue button in the bottom left-hand corner to open a Remote Window
7. In the text box that appears, select "Connect to Host..."
8. In the text box, type `nvidia@<IP>` where <IP> should be substituted for your Nano's IP address
9. Enter the Nano's password to confirm (this password is usually `nvidia`)
10. Click on the page icon in the top left to open the Explorer and click "Open Folder"
12. In the text box that appears, type `/home/nvidia/` and press enter
13. Enter the password to confirm

You have successfully connected to your Jetson Nano!

### Part Two: Downloading and Organizing Datasets
1. Download two datasets on Kaggle, found [here](https://www.kaggle.com/datasets/yinchuangsum/person-wheel-chair-not-wheel-chair) and [here](https://www.kaggle.com/datasets/constantinwerner/human-detection-dataset)
2. Extract the contents of both .zip files on to your computer
3. Reopen the Start menu and type `FileZilla`
4. Click on the Application link that appears
5. In the row of text boxes at the top, enter your Nano's IP into the "Host:" box, "nvidia" into both the "Username" and "Password" boxes, and "22" in the "Port box
6. Press "Quickconnect" to allow your computer to transfer files to and from your Nano
7. In the "Remote site" box on the right, navigate to `/home/nvidia/jetson-inference/python/training/classification/data`
8. Create a new directory titled `wheelchair_detector` inside "data"
9. Within "data", create three other directories: `test`, `train`, and `val`
10. Within each of these three directories, create three subdirectories: `handicapped`, `non-handicapped`, and `none`
11. From the "Wheelchair" dataset, drag and drop all image files (except for those prefixed with "bike") from its subfolders to the corresponding Nano directories within the "handicapped" directory using FileZilla
13. Within the "Humans" dataset, split the images in the "0" folder into three portions--transfer 80% into "train/none" and 10% each into "test/none" and "val/none"
14. Repeat the above step for the "1" folder, substituting "/none" for "/non-handicapped"
15. Create a new file within "data/handicapped_none" called `labels.txt`
16. Within VS Code's File Explorer, navigate to "jetson-inference/python/training/classification/data/wheelchair_detector" and open "labels.txt"
17. Add `handicapped`, `not-handicapped`, and `none` to the text file, each on their own line and in that exact order. Save the file

Congratulations, you have finished setting up the data your AI will be trained on!

### Part Three: Training the Model
1. Using the VS Code Terminal window, navigate to "jetson-inference" (`cd jetson-inference`)
5. Type `./docker/run.sh` to enter the docker
6. Navigate to "jetson-inference/python/training/classification" (`cd python/training/classification`)
7. Type `python3 train.py --model-dir=models/wheelchair_dectector data/wheelchair_dectector` to train the model. This can take a while! (optional arguments include: `--batch-size=<8>`, `--workers=<2>`, and `--epochs=<35>`, where <#> represents default values)
8. Type `python3 onnx_export.py --model-dir=models/wheelchair_detector` to export your model
9. Press `Ctrl + D` to exit the docker

Your AI model is now ready for use!

### Part Four: Running the Model
1. Navigate to "jetson-inference/python/training/classification"

Manual, single-image use:

1. Type `NET=models/wheelchair_detector` to set the NET variable
2. Type `DATASET=data/wheelchair_detector` to set the DATASET variable
3. Type `imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt <input> <output>` where <input> is the relative path of the photo to be analyzed and <output> is the relative path of the to-be exported photo

The analyzed image will be viewable at your designated path!

Continuous, webcam-enabled use:

1. From this Github page, download "auto-door-opener.py"
2. Using FileZilla, place the .py file in "jetson-inference/python/training/classification"
3. In VS Code's Terminal, navigate to "jetson-inference/python/training/classification"
4. Type `python3 auto-door-opener.py` to initialize the program
5. When finished, press `Ctrl + C` to end the process

This concludes the project's instructional tutorial!
