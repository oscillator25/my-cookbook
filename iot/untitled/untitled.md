# TINA, Learns About Face-Recognition Using OpenCV And Python3.

So, here it is how it begins, TINA- the other augmented reality bot was just made to life and is introduced to the augmented world of “Qubit” and his first bot, MARK. MARK likes to help and teach others out of his best knowledge and got rejoiced seeing TINA that he has someone to brainstorm-with.

TINA was engrossed with “Reinforcement-Learning” and thereby had a great ability to learn from her environment. It is evident in her behavior, too, always eager to learn something new.” Qubit,” knowing MARK’s helping nature, asked him to help TINA clear her doubts and learn new things.

&lt;After Qubit’s departure&gt;

**MARK**: “Hey, Myself, Mark, nice to meet you.”

**TINA**: “Nice to meet you, too; I am Tina.”

**MARK**: “So, Tina, what you’ll like to learn for today?”

**TINA**: “Hmm, I am not sure what interesting I can try with my present knowledge.”

**MARK**: “Ok, No problem. Just let me know what all you know presently.”

**TINA**: “You don’t worry about my current knowledge. I will just ask Qubit to add the prerequisites needed to my “Watson Discovery” Service.”

**MARK**: “Ok. Cool. So, Why not we just start from the project from which Qubit himself started his learning journey. The project is about a face-recognition surveillance service being developed using Python3 and openCV library named Vision“

**TINA**: “Yes, That’s a good idea.”

**MARK**: “ So, here, the learning begins. We would be talking about a face-recognition OPENCV project.”

**TINA**: “Great! I love exploring the realm of Computer-Vision.”

**MARK**: “Let’s get started then. This application is designed to detect and authenticate a person and notify him/her if some intruder is caught in the concerned area and then he/she will be notified of this with a picture of an intruder being sent to the predefined e-mail. It is intended to demonstrate how to use CV and IOT to improve and create a facial\_recognition authenticator. Let’s talk a bit about the requirements.”

## Requirements <a id="96e3"></a>

### Hardware <a id="a353"></a>

* BOLT-IOT

### Software <a id="888b"></a>

* [Ubuntu\* 18.04 LTS](http://releases.ubuntu.com/18.04/)
* OpenCL™ Runtime package
* _Note_: We recommend using a 4.14+ kernel to use this software. Run the following command to determine your kernel version:

```text
uname -a
```

### **MARK: “Here’s an outline of the code-pattern.”** <a id="f713"></a>

**MARK**: “Now as you have a basic idea of what all to do, let’s delve deep into the code…”

**TINA**: “Sure, I’m ready for it.”

### MARK: “First of all you need to figure out what all libraries and packages are required.” <a id="95e4"></a>

```text
import cv2import numpy as npimport requestsimport dlibimport imutilsimport face_recognitionfrom boltiot import Boltfrom scipy.spatial import distance as distimport timeimport econffrom imutils import face_utilsfrom imutils.video import FileVideoStreamfrom imutils.video import VideoStream
```

TINA: “Can you provide me some reference to study for these libraries? I will then ask Qubit to update it in my database.”

MARK: “Sure, but let me check if I have some reference or other stuff for you to study. Oh! sorry but this time I am not having any reference though you can Google it, I think you have the webhook capability.”

TINA: “Yup, I can Google of course but wouldn't it be nice if Qubit just gives us some material. I’m thinking to ask him for it.”

### MARK: “Now after figuring out all the libraries and packages, you need to decide which model is to be used, the selection of the model is solely dependent upon your purpose.” <a id="26e7"></a>

```text
prediction_model = "shape_predictor_68_face_landmarks.dat"
```

### MARK: “Next, you will require to collect some API credentials for alerting services. For [BOLT-IOT](https://cloud.boltiot.com/) and [Mailgun](https://www.mailgun.com/) visit their respective consoles and copy their credentials as shown below. You can also use services like [Sendgrid](https://sendgrid.com/) in place of Mailgun and in case you want to use Raspberry pi instead of the bolt it, can do it using node-red, its something we will cover in another tutorial.” <a id="a169"></a>

```text
from boltiot import Boltapi_key = "<Enter your API Key>"device_id  = "<Enter Device ID>"mybolt=Bolt(api_key, device_id)
```

### MARK: “After doing all this stuff comes the real calculation part of calculating the eye-aspect ratio. Here you are required to have knowledge about euclidean concepts.” <a id="28ce"></a>

### **TINA**: “I’m planning to brush up my concepts then using the openCV doc before moving forward.” <a id="db9b"></a>

### MARK: ” Ya, sure it’s required. Here’s the [link](https://docs.opencv.org/master/d6/d00/tutorial_py_root.html).” <a id="1556"></a>

### TINA: “It’s done let’s continue.” <a id="554f"></a>

### MARK: “Calculate the eye-aspect ratio.” <a id="280c"></a>

```text
def eye_aspect_ratio(eye):# computing the euclidean distances between the two sets of vertical eye landmarks (x, y)-coordinatesA = dist.euclidean(eye[1], eye[5])B = dist.euclidean(eye[2], eye[4]# computing the euclidean distance between the horizontal eye landmark (x, y)-coordinatesC = dist.euclidean(eye[0], eye[3])# computing the eye-aspect ratioear = (A + B) / (2.0 * C)# return the eye-aspect ratioreturn ear
```

### MARK: “Now, Do a bit of initialization.” <a id="3704"></a>

```text
# define two constants, one for the eye aspect ratio to indicate blink and then a second constant for the number of consecutive frames the eye must be below the thresholdEYE_AR_THRESH = 0.3EYE_AR_CONSEC_FRAMES = 3# initialize the frame counters and the total number of blinksCOUNTER = 0TOTAL = 0# initialize dlib's face detector (HOG-based) and then create the facial landmark predictorprint("Loading...")detector = dlib.get_frontal_face_detector()predictor = dlib.shape_predictor(prediction_model)
```

### MARK: “Now let’s perform some little but important tasks.” <a id="6b0d"></a>

```text
# grab the indexes of the facial landmarks for the left and right eye, respectively(lStart, lEnd) = face_utils.FACIAL_LANDMARKS_IDXS["left_eye"](rStart, rEnd) = face_utils.FACIAL_LANDMARKS_IDXS["right_eye"]# Read the users data and create face encodingsimage = face_recognition.load_image_file("Image(mine).jpg")mface_encoding = face_recognition.face_encodings(image)[0]known_names = ['Pratyaksh']known_encods = [mface_encoding]# start the video stream threadprint("Starting Video Stream")fileStream = Truevs = VideoStream(src='<Enter URL To IP Webcam Stream>').start()time.sleep(1.0)print("Video Stream Started")face_locations = []face_encodings = []face_names = []
```

### MARK: “Next, comes the looping over the frame, in the same way, your intents in Watson Speech Service are looped over and over again searching for the intent.” <a id="036c"></a>

```text
# loop over frames from the video streamwhile True:# grab the frame from the threaded video file stream, resize it, and convert it to grayscale channelsframe = vs.read()frame = imutils.resize(frame, width=1080)gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)small_frame = cv2.resize(rgb, (0, 0), fx=0.25, fy=0.25)
```

### MARK: “Here, comes the face identification part. First of all, you need to find all faces and provide an encoding to each. Encoding is just like a label to each face detected by the classifier in the frame.” <a id="b60f"></a>

```text
# Find all the faces and face encodings in the (current) video frameface_locations = face_recognition.face_locations(small_frame)face_encodings = face_recognition.face_encodings(small_frame, face_locations)name = "Unknown"face_names = []for face_encoding in face_encodings:match = face_recognition.compare_faces(known_encods, face_encoding)if match[0]:name = known_names[0]face_names.append(name)# detect faces in the grayscale framerects = detector(gray, 0)
```

### MARK: “Now loop over the face detections. During this process, we put-in the counting of eye-blinking so as to differentiate a picture from a real person. Also, the eye-aspect ratio which was calculated earlier is used to compare between the picture\(which is a 2-d plot\) and real person in front of webcam\(a 3-d plot\), leading to 2-way authentication first by means of considering blinking and second by using eye-aspect ratio, thus giving better and accurate results.” <a id="9858"></a>

```text
# loop over the face detectionsfor rect in rects:# determine the facial landmarks for the face region, then convert the facial landmark (x, y)-coordinates to a numpy arrayshape = predictor(gray, rect)shape = face_utils.shape_to_np(shape)# extract the left and right eye coordinates, then use the coordinates to compute the eye aspect ratio for both eyesleftEye = shape[lStart:lEnd]rightEye = shape[rStart:rEnd]leftEAR = eye_aspect_ratio(leftEye)rightEAR = eye_aspect_ratio(rightEye)# average the eye aspect ratio together for both eyesaspect_ratio = (leftEAR + rightEAR) / 2.0# compute the convex hull for the left and right eye, then visualize each of the eyesleftEyeHull = cv2.convexHull(leftEye)rightEyeHull = cv2.convexHull(rightEye)cv2.drawContours(frame, [leftEyeHull], -1, (0, 255, 0), 1)cv2.drawContours(frame, [rightEyeHull], -1, (0, 255, 0), 1)# check to see if the eye aspect ratio is below the blink threshold, and if so, increment the blink frame counterif aspect_ratio < EYE_AR_THRESH:COUNTER += 1# otherwise, the eye aspect ratio is not below the blink thresholdelse:# if the eyes were closed for a sufficient number of then increment the total number of blinksif COUNTER >= EYE_AR_CONSEC_FRAMES:TOTAL += 1# reset the eye frame counterCOUNTER = 0
```

### MARK: “Next, in case you want to display the total number of blinks encounter by the system, use the code snippet given below.” <a id="23c9"></a>

```text
# draw the total number of blinks on the frame along with the computed eye aspect ratio for the framecv2.putText(frame, "NO. OF BLINKS : {}".format(TOTAL), (10, 30),cv2.FONT_HERSHEY_DUPLEX, 0.9, (0, 0, 255), 2)cv2.putText(frame, "EYE-ASPECT RATIO : {:.2f}".format(aspect_ratio), (300, 30),cv2.FONT_HERSHEY_DUPLEX, 0.9, (0, 0, 255), 2)
```

### MARK: “Next, display the results. The box is drawn around the face of the person with the respective encodings in case of the owner and ‘intruder’ next to the name in case the person’s face fails to match.” <a id="b29e"></a>

```text
# Display the resultsfor (top, right, bottom, left), name in zip(face_locations, face_names):# Scale back up face locations since the frame we detected in was scaled to 1/4 sizetop *= 4right *= 4bottom *= 4left *= 4if face_names[0]!='Unknown':while TOTAL>=3:# Draw a box around the facecv2.rectangle(frame, (left, top), (right, bottom), (0, 0, 255), 2)font = cv2.FONT_HERSHEY_DUPLEX# Draw a label with a name below the facecv2.rectangle(frame, (left, bottom - 35),(right, bottom), (0, 0, 255), cv2.FILLED)cv2.putText(frame, name, (left + 6, bottom - 6),font, 1.0, (255, 255, 255), 1)cv2.putText(frame, 'Authorized!!!', (frame.shape[1]//2, frame.shape[0]//2), font, 1.0, (0, 255, 0), 1)breakelse:font = cv2.FONT_HERSHEY_DUPLEXcv2.putText(frame, 'Intruder!!!', (frame.shape[1]//2, frame.shape[0]//2), font, 2.0, (0, 0, 255), 1)cv2.imwrite('Intruder.jpg',frame[right:right+left,top:top+bottom])print("\n[CAUTION] Sending alert to Pratyaksh")
```

### MARK: “Here, comes the part to activate the buzzer in case image-authentication fails. The buzzer is triggered using the Bolt\_IOT module and its cloud service. The response is set to ‘HIGH’ i.e. buzzer is activated.” <a id="5428"></a>

```text
response=mybolt.digitalWrite('0','HIGH')print("\n [CAUTION] Sending e-mail to Pratyaksh")
```

### MARK: “In order to receive the e-mail with the intruder’s image the API service needs to be authenticated which in case of Mailgun service can be done as shown below. You can also customize the message that the owner should receive by making changes to respective fields below.” <a id="7017"></a>

```text
# creating mailgun API calldef send_simple_message():return requests.post("<https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/messages>",auth=("api", "<Enter Mailgun API Key>"),data={"from": "Excited User <mailgun@YOUR_DOMAIN_NAME>","to": "<Enter Reciever's Email>","subject": "Intruder Detected","text": "Someone tried to access your application","html":'<html>Inline image here:<img src="cid:opencv_Intruder.jpg"></html>'})
```

### MARK: “Using the below command the buzzer can be set to default state.” <a id="311b"></a>

```text
response=mybolt.digitalWrite('0','LOW')
```

### MARK: “For displaying the frame use the below commands.” <a id="16b7"></a>

```text
# show the framecv2.imshow("Frame", frame)key = cv2.waitKey(1) & 0xFF
```

### MARK: “You can initialize a key\(here ‘q’ is initialized\) to exit.” <a id="5a8f"></a>

```text
# if the `q` key was pressed, break from the loopif key == ord("q"):break
```

### MARK: ”It’s always nice to do a bit of cleanup.” <a id="d7bf"></a>

### TINA: ”Ok, Cool.” <a id="7dc8"></a>

```text
# do a bit of cleanupprint("\n Recognition Task Accomplished")cv2.destroyAllWindows()vs.stop()
```

**TINA**: “I can’t wait for more to try implementing it, but in case I get stuck or need to look at the code; can you please provide me the link to the repository of this project. I promise wouldn't;t directly copy and paste from there but will use the instructions you had provided in creating this project.”

**MARK**: “ Ok, if you need it for reference only then I’ll provide the link to the project’s repository on Github. Here’s the [**link**](https://github.com/oscillator25/Vision). **Happy Coding!!!**”

