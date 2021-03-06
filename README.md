---------------------------------------------------------------------------------------------------
# Realtime Face Recognition

Face recognition problems commonly fall into two categories:

 * Face Verification - "is this the claimed person?". For example, at some airports, you can pass through customs by letting    a system scan your passport and then verifying that you (the person carrying the passport) are the correct person. A          mobile phone that unlocks using your face is also using face verification. This is a 1:1 matching problem.
 * Face Recognition - "who is this person?". This is a 1:K matching problem.
 
FaceNet learns a neural network that encodes a face image into a vector of 128 numbers. By comparing two such vectors, you can then determine if two pictures are of the same person.

--------------------------------------------------------------------------------------------------
### Encoding face images into a 128-dimensional vector

--------------------------------------------------------------------------------------------------
#### Using an ConvNet to compute encodings


The FaceNet model takes a lot of data and a long time to train. So following common practice in applied deep learning settings, let's just load weights that someone else has already trained. The network architecture follows the Inception model from [Szegedy et al.](https://arxiv.org/abs/1409.4842). We have provided an inception network implementation. You can look in the file inception_blocks.py

The key things you need to know are:

 * This network uses 96x96 dimensional RGB images as its input. Specifically, inputs a face image (or batch of m face       images) as a tensor of shape (m, n_C, n_H, n_W) = (m, 3, 96, 96)
 * It outputs a matrix of shape (m, 128) that encodes each input face image into a 128-dimensional vector

--------------------------------------------------------------------------------------------------
#### Expected Output

By using a 128-neuron fully connected layer as its last layer, the model ensures that the output is an encoding vector of size 128. You then use the encodings the compare two face images as follows:


<p align="center"><img width="100%" src="https://github.com/SHANK885/realtime_face_recognition/blob/master/images/distance_kiank.png" /></p>

So, an encoding is a good one if:

 * The encodings of two images of the same person are quite similar to each other
 * The encodings of two images of different persons are very different

The triplet loss function formalizes this, and tries to "push" the encodings of two images of the same person (Anchor and Positive) closer together, while "pulling" the encodings of two images of different persons (Anchor, Negative) further apart.

<p align="center"><img width="100%" src="https://github.com/SHANK885/realtime_face_recognition/blob/master/images/triplet_comparison.png" /></p>


--------------------------------------------------------------------------------------------------
### Platform Secification:

  * Ubuntu 18.04


--------------------------------------------------------------------------------------------------
### Requirements:
  
  * tensorflow==1.15.0
  * sklearn==0.21.3
  * Python==3.7.4
  * OpenCV==4.1.2
  * NumPy==1.17.2
 
 
-------------------------------------------------------------------------------------------------- 
### Setup

  * Clone this [repository](https://github.com/SHANK885/realtime_face_recognition.git)
  
  
--------------------------------------------------------------------------------------------------  
### Enroll a new face using webcam.

  1. Go inside realtime_face_recognition directory.
  2. run "python enroll_face.py <name_of_new_member>
  3. Webcam will open up with a window to capture face.
  4. Press 's' by selectiong the video window to capture the image.
  5. If you want to recapture the image:
        select the terminal window and enter "R" or "r" else enter "C" or "c".

  It will enroll the new face with the name provided in the command line.

  
  The cropped and aligned face will be saved to:
        realtime_face_recognition/database/images/ directory
  
  The 128 D face embedding vector will be added to:
        realtime_face_recognition/database/embeddings/face_embeddings.json


--------------------------------------------------------------------------------------------------
### Where the image is stored ?

  * The cropped faces of all te enrolled members is stored in:
    [realtime_face_recognition/database/images/ directory](https://github.com/SHANK885/realtime_face_recognition/tree/master/database/images)
  * The embeddings of all the enrolled faces is present in:
    [realtime_face_recognition/database/embeddings/<emb> directory](https://github.com/SHANK885/realtime_face_recognition/tree/master/database/embeddings)
  
  
--------------------------------------------------------------------------------------------------
### What is does?

Our realtime face recognition is able to recognize the faces of all the members that is enrolled in the database. However, if a face is not enrolled it will make it as unknown.


--------------------------------------------------------------------------------------------------
### How to run FaceNet Realtime Recognition.

  * Enroll the faces you want by following the above steps.
  * Go to the realtime_face_recognition directory.
  * run realtime_recognition.py.
  * It will be able to recognize the faces that are present in the database and will mark a face unknown if it is not             registered.
  
--------------------------------------------------------------------------------------------------
### References

 * Florian Schroff, Dmitry Kalenichenko, James Philbin (2015). [FaceNet: A Unified Embedding for Face Recognition and Clustering](https://arxiv.org/pdf/1503.03832.pdf)
 * Yaniv Taigman, Ming Yang, Marc'Aurelio Ranzato, Lior Wolf (2014). [DeepFace: Closing the gap to human-level performance in face verification](https://research.fb.com/wp-content/uploads/2016/11/deepface-closing-the-gap-to-human-level-performance-in-face-verification.pdf)
 * The pretrained model we use is inspired by Victor Sy Wang's implementation and was loaded using his code: [https://github.com/iwantooxxoox/Keras-OpenFace](https://github.com/iwantooxxoox/Keras-OpenFace)
 * Our implementation also took a lot of inspiration from the official FaceNet github repository: [https://github.com/davidsandberg/facenet](https://github.com/davidsandberg/facenet)
