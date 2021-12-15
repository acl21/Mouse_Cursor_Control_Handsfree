# Mouse Cursor Control Using Facial Movements [![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://github.com/akshaychandra21/Mouse_Cursor_Control_Handsfree/blob/master/LICENSE)

This HCI (Human-Computer Interaction) application in Python(3.6) will allow you to control your mouse cursor with your facial movements, works with just your regular webcam. Its hands-free, no wearable hardware or sensors needed.

At this point, you are forced to work with the facial movements I chose but I am working on making them configurable. The list of actions include:

 - Squinting your eyes (**squint** - To look with the eyes partly closed, as in bright sunlight)
 - Winking
 - Moving your head around (pitch and yaw)
 - Opening your mouth (a little bit, yes)

Special thanks to **Adrian Rosebrock** for his amazing blog posts [[2](#references)] [[3](#references)], code snippets and his imutils library [[7](#references)] that played an important role in making this idea of mine a reality.

## Working Example
<img src="https://github.com/akshaychandra21/Mouse_Cursor_Control_Handsfree/blob/master/demo.gif">

## Code Requirements
* Numpy - 1.13.3
* OpenCV - 3.2.0
* PyAutoGUI - 0.9.36
* Dlib - 19.4.0
* Imutils - 0.4.6

## Execution
Order of Execution is as follows:

1. Follow these installation guides - [Numpy](https://pypi.org/project/numpy/), [OpenCV](https://medium.com/@akshaychandra21/f5f721f0d0b3), [PyAutoGUI](https://pyautogui.readthedocs.io/en/latest/install.html), [Dlib](https://www.learnopencv.com/install-opencv-3-and-dlib-on-windows-python-only/), [Imutils](https://github.com/jrosebr1/imutils) and install the right versions of the libraries (mentioned above).
2. Make sure you have the model downloaded. Read the README.txt file inside the model folder for the link. 
3. `python mouse-cursor-control.py`

Please raise an issue in case of any errors. 

## Usage
 
I definitely understand that these facial movements could be a little bit weird to do, especially when you are around people. Being a patient of [benign-positional-vertigo](https://www.healthline.com/health/benign-positional-vertigo), I hate doing some of these actions myself. But I hope to make them easier and less weird over time. Feel free to suggest some public friendly actions that I can incorporate in the project. 

<div align="center">
<img src="images/usage.jpg" width=360 height=460/>
</div>

## How It Works
This project is deeply centered around predicting the facial landmarks of a given face. We can accomplish a lot of things using these landmarks. From detecting eye-blinks [[3](#references)] in a video to predicting emotions of the subject. The applications, outcomes and possibilities of facial landmarks are immense and intriguing.

[Dlib](dlib.net/)'s prebuilt model, which is essentially an implementation of [[4](#references)], not only does a fast face-detection but also allows us to accurately predict 68 2D facial landmarks. Very handy.  

<div align="center">
<img src="images/facial-landmarks-68.jpg" width=500 height=190/>
</div>

Using these predicted landmarks of the face, we can build appropriate features that will further allow us to detect certain actions, like using the eye-aspect-ratio (more on this below) to detect a blink or a wink, using the mouth-aspect-ratio to detect a yawn etc or maybe even a pout. In this project, these actions are programmed as triggers to control the mouse cursor. [PyAutoGUI](http://pyautogui.readthedocs.io) library was used to control the mouse cursor. 

### Eye-Aspect-Ratio (EAR)
You will see that Eye-Aspect-Ratio [[1](#references)] is the simplest and the most elegant feature that takes good advantage of the facial landmarks. EAR helps us in detecting blinks [[3](#references)] and winks etc.  
<div align="center">
<img src="images/EAR-final.png" width=772 height=298/>
</div>

You can see that the EAR value drops whenever the eye closes. We can train a simple classifier to detect the drop. However, a normal if condition works just fine. Something like this:

    if EAR <= SOME_THRESHOLD:
       EYE_STATUS = 'CLOSE'
    
### Mouth-Aspect-Ratio (MAR)
Highly inspired by the EAR feature, I tweaked the formula a little bit to get a metric that can detect open/closed mouth. Unoriginal but it works.  

<div align="center">
<img src="images/MAR-final.jpg" width=700 height=300/>
</div>

Similar to EAR, MAR value goes up when the mouth opens. Similar intuitions hold true for this metric as well. 

## Prebuilt Model Details

The model offers two important functions. A detector to detect the face and a predictor to predict the landmarks. The face detector used is made using the classic Histogram of Oriented Gradients (HOG) feature combined with a linear classifier, an image pyramid, and sliding window detection scheme. 

The facial landmarks estimator was created by using dlib's implementation of the paper:
[One Millisecond Face Alignment with an Ensemble of Regression Trees by
      Vahid Kazemi and Josephine Sullivan, CVPR 2014](https://www.semanticscholar.org/paper/One-millisecond-face-alignment-with-an-ensemble-of-Kazemi-Sullivan/1824b1ccace464ba275ccc86619feaa89018c0ad). 
And was trained on the iBUG 300-W face landmark dataset: C. Sagonas, E. Antonakos, G, Tzimiropoulos, S. Zafeiriou, M. Pantic. 300 faces In-the-wild challenge: Database and results. [Image and Vision Computing (IMAVIS), Special Issue on Facial Landmark Localisation "In-The-Wild". 2016](https://ibug.doc.ic.ac.uk/resources/facial-point-annotations/).

<div align="center">
<img src="images/faces.png" width=870 height=330/>
</div>

You can get the trained model file from http://dlib.net/files, click on **shape\_predictor\_68\_face\_landmarks.dat.bz2**. The model dat file has to be in the model folder.

Note: The license for the iBUG 300-W dataset excludes commercial use. So you should contact Imperial College London to find out if it's OK for you to use this model file in a commercial product.

## References
- **[1]**. Tereza Soukupova´ and Jan Cˇ ech. _[Real-Time Eye Blink Detection using Facial Landmarks](https://vision.fe.uni-lj.si/cvww2016/proceedings/papers/05.pdf)_. In 21st Computer Vision Winter Workshop, February 2016.

- **[2]**. Adrian Rosebrock. _[Detect eyes, nose, lips, and jaw with dlib, OpenCV, and Python](https://www.pyimagesearch.com/2017/04/10/detect-eyes-nose-lips-jaw-dlib-opencv-python/)_. 

- **[3]**. Adrian Rosebrock. _[Eye blink detection with OpenCV, Python, and dlib](https://www.pyimagesearch.com/2017/04/24/eye-blink-detection-opencv-python-dlib/)_.

- **[4]**. Vahid Kazemi, Josephine Sullivan. _[One millisecond face alignment with an ensemble of regression trees](https://ieeexplore.ieee.org/document/6909637)_. In CVPR, 2014.

- **[5]**. S. Zafeiriou, G. Tzimiropoulos, and M. Pantic. _[The 300 videos in the wild (300-VW) facial landmark tracking in-the-wild challenge](http://ibug.doc.ic.ac.uk/resources/300-VW/.3)_. In ICCV Workshop, 2015. 

- **[6]**. C. Sagonas, G. Tzimiropoulos, S. Zafeiriou, M. Pantic. _[300 Faces in-the-Wild Challenge: The first facial landmark localization Challenge](https://ibug.doc.ic.ac.uk/media/uploads/documents/sagonas_iccv_2013_300_w.pdf)_. Proceedings of IEEE Int’l Conf. on Computer Vision (ICCV-W), 300 Faces in-the-Wild Challenge (300-W). Sydney, Australia, December 2013

- **[7]**. Adrian Rosebrock. *Imutils*. [https://github.com/jrosebr1/imutils](https://github.com/jrosebr1/imutils).

- **[8]**. Akshay Chandra Lagandula. *Mouse Cursor Control Using Facial Movements*. [https://towardsdatascience.com/c16b0494a971](https://towardsdatascience.com/c16b0494a971).
