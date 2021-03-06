# LEGOBrickRecognition
Using convolutional neural network (cNN) for recognition of small items like LEGO bricks.

For science and study it can be a time consuming task to build datasets designated for training cNN. Some public data sets exist like MNIST (hand written digits) and those are used in many cases for testing new algorithms or neural network models. However, these public datasets are often hard to split into smaller subsets for study specific areas of image recognition and classification.

My focus is to make it easy to generate images that can be used for training cNN. The requirements to the quality of images are:

1) Capture image of item from a random angle
2) Enough light to identify even minor differences
3) White light to catch proper colors
4) Non-reflective background to avoid refections / "corona" around items.

### Solution in short
ad 1)
The first requirement can be solved by constructing a machine that can flip an item into the air -- to avoid dropping the item this could be done within a box.

ad 2+3)
The lighting is a bit tricky as the items (e.g. LEGO bricks) come in a lot of different colors. Using white-foam for background did produce a random pattern as background (kind of noise which is good)

ad 4)
However, the trained cNN captured additional lighting around the items as the colors were reflected onto the white background. This actually helped the cNN in the inference phase and increased recognitions of even very similar items. However, when trying to recognize items in another environment (outside the training area) then the surrounding reflection where missing resulted in lower prediction score. Removing the "corona" around the items is important to be able to use the images for training. Manually processing each image to cut the item free of surrounding background is time consuming and it is there important to choose a material that is as non-reflective as possible.

(The machine must be able to change the background to handle items of many different colors. For example use black for all non-gray, black or dark items. Those items may be captured by using a green background. By splitting the frame grabbing into two background colors ease the task of automating the image grabbing. In practice it requires two boxes of different colors -- in theory, a semi-transparent box surrounded by led lights could change the color from white to green, however, at a risk of getting reflection.)


### Low Cost solutions

The solution should be low cost and make it affordable for students at all levels.


Over all process for grabbing frames of items:

1) Receive item one-by-one from a source (e.g. from a conveyor belt)
2) Flip an item in the air multiple times
3) Eject item after processing
4) Continue with next item

The speed depends upon the camera quality and lightning + the time for getting items into and out of the box.
With high speed camera it is possible to grab many images per second, however, if the images to too similar there is a risk in over-training the cNN. It is more import to get a camera that has a good color depth. For example my current prototype grabs only 5 frame per second.

Use whatever material you have at hand. My prototype is built from LEGO bricks, however, that is not at focus here.

Using this prototype I have been able to grab 17.000 images of 28 different LEGO bricks in less than two hours "


## Setup

Lets get to the technical setup. I have used the following hardware and software:

### Hardware
Pre-processing of images and learning:
NVIDIA gtx 1080

Capture of image and cNN inference (real-time evaluation of camera input) are done at:
NVIDIA Jetson TX1 (at developer board)

Camera:
On-board camera at the TX1 developer board.


### Software

To get started read the very good guide written by Dustin Franklin at NVIDIA corporation.
https://github.com/dusty-nv/jetson-inference

If you prefer the Docker way then a really easy install guide is found here:
https://github.com/NVIDIA/nvidia-docker/wiki/DIGITS

After installing Digits you may return to the guide written by Dustin (see "Importing Classification Dataset into DIGITS")

Until now the utility imagemagick has been used for pre-processing of images.

### Machine

LEGO bricks
LEGO Mindstorm EV3 (for motor control, however, this is very simple controls that could be managed by any I2C device)
One motor for hammering at the buttom of the box to make the item flip in the air.
One large EV3 motor for tilting the box and thereby dropping the contained item.
One small EV3 motor for running the conveyor belt
LEGO Train 9V transformer to minimize battery usage
White foam plates (just reused material from some package delivery)

I have attached a conveyor belt to the solution so the process can run unattended for some time. This is an area where I probably should look at others solutions for separating one LEGO brick from others and thereby feeding the box with one brick at a time.

My solution is build from what I had at hand -- I would recommend looking into some more compact design using a magnet and spool (like a doorbell) for hammering at the buttom of the box.

Getting the item out of the box is a bit tricky; in my case I tilt or lift the box and thereby letting the item slide out of it.
If the box is build from some acryl plates with a hinge on one of the sides then that would be more elegant.
When it comes to it you want the item to get out of the box fast to get on with the next item.

### Process

Capturing 5 images per second for 1 minut gives 300 images for each item.
Gathering verification and test images as well will increase the recording periode (getting 25% for verification and 10% for test).

If you use the raw images for training the network then you may end up with a cNN that only recognize items in the same surrounding.
Therefore my next step is to look into training an object detection cNN which can identify the item and then feed that smaller area into the classification training.



## Disclamer:
"LEGO®is a trademark of the LEGO Group of companies which does not sponsor, authorize or endorse this project"


