# Hand_House_Control

This program is meant to be a repersentaion of what A.I. can do in assistance to everyday life; such as turning on and off ones house lights with only a raise of the finger.
this program is still being worked on!
---


## The Algorithm

Add an explanation of the algorithm and how it works. Make sure to include details about how the code works, what it depends on, and any other relevant info. Add images or other descriptions for your project here.
This program is an A.I. detection model that is trained off of data files which can be in the form of text, image or video. Within this model it is trained off of 900 pictures of a vast varity of different hand poses which the A.I. can learn which hand sign means what.

## Running this project

[View a video explanation here]([video link](https://youtu.be/WhE-RXKazd8))

### Step One: Connecting to your Jetson Nano
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

### Step Two: Downloading and Organizing Datasets
1. Download two datasets on Kaggle with fingers that you find or take your own
2. Extract the contents of both .zip files on to your computer
3. Reopen the Start menu and type `FileZilla`
4. Click on the Application link that appears
5. In the row of text boxes at the top, enter your Nano's IP into the "Host:" box, "nvidia" into both the "Username" and "Password" boxes, and "22" in the "Port box
6. Press "Quickconnect" to allow your computer to transfer files to and from your Nano
7. In the "Remote site" box on the right, navigate to `/home/nvidia/jetson-inference/python/training/classification/data`
8. Create a new directory titled `wheelchair_detector` inside "data"
9. Within "data", create three other directories: `test`, `train`, and `val`
10. Within each of these three directories, create three subdirectories: `handicapped`, `non-handicapped`, and `none`
11. From the "houseHand" dataset, drag and drop all image files (except for those prefixed with "bike") from its subfolders to the corresponding Nano directories within the "finger1" and "finger2" directory using FileZilla
13. Within the "Humans" dataset, split the images in the "0" folder into three portions--transfer 80% into "train/none" and 10% each into "test/none" and "val/none"
14. Repeat the above step for the "1" folder, substituting "/none" for "/non-handicapped"
15. Create a new file within "data/handicapped_none" called `labels.txt`
16. Within VS Code's File Explorer, navigate to "jetson-inference/python/training/classification/data/wheelchair_detector" and open "labels.txt"
17. Add `finger1`, `finger2`, and `empty` to the text file, each on their own line and in that exact order. Save the file

Congratulations, you have finished setting up the data your AI will be trained on!

### Step Three: Training the Model
1. Using the VS Code Terminal window, navigate to "jetson-inference" (`cd jetson-inference`)
5. Type `./docker/run.sh` to enter the docker, entering the password (usually `nvidia`) when prompted
6. Navigate to "jetson-inference/python/training/classification" (`cd python/training/classification`)
7. Type `python3 train.py --model-dir=models/houseHand data/houseHand` to train the model. This can take a while! (optional arguments include: `--batch-size=<8>`, `--workers=<2>`, and `--epochs=<35>`, where <#> represents default values)
8. Type `python3 onnx_export.py --model-dir=models/houseHand` to export your model
9. Press `Ctrl + D` to exit the docker

Your AI model is now ready for use!

### Step Four: Running the Model
1. Navigate to "jetson-inference/python/training/classification"

Manual, single-image use:

1. Type `NET=models/houseHand` to set the NET variable
2. Type `DATASET=data/houseHand` to set the DATASET variable
3. Type `imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt <input> <output>` where <input> is the relative path of the photo to be analyzed and <output> is the relative path of the to-be exported photo

The analyzed image will be viewable at your designated path!

Continuous, webcam-enabled use:

1. From this Github page, download "Hand_House_Conrtol.py"
2. Using FileZilla, place the .py file in "jetson-inference/python/training/classification"
3. In VS Code's Terminal, navigate to "jetson-inference/python/training/classification"
4. Type `python3 houseHand.py` to initialize the program
5. When finished, press `Ctrl + C` to end the process

This concludes the project's instructional tutorial!
credit to StrangeQuark0
