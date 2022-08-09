# DeepUnfolding


## `Research Big Picture`

Order | Problem Solving PLAN
:--------------- | :--------------- 
(1)   | `Check` the created dataset(alignment.h5, mri.h5) from DeCoLearn
(2)   | Plan how to `compose multiple coils dataset`, and code it
(3)   | `Put Deep Unfolding Module` into DeCoLearn
(4)   | `train` two models: EDSR DeCoLearn and Deep Unfolding DeCoLearn
(5)   | `Revise config.json` file. I added boolean variable mul_coil.

Final  | Compare between basic DeCoLearn trained with multiple coils data with unfolding DeCoLearn trained with multiple coils data

----

## [1] `Dataset`

### [1-1] `Orginal DeCoLearn Dataset Structure`

`source_h5`  	     |      Shape
:---------------:   | :---------------:
x                   | (524, 2, 256, 232)


`alignment_h5`  	     |      Shape
:---------------:   | :---------------:
moved_x             | (524, 2, 256, 232)

`mri_h5`  	                    |      Shape
:---------------:             | :---------------:
fixed_y                       |  (524, 256, 232, 2)
fixed_mask                    |  (524, 256, 232, 2)
moved_y                       |  (524, 256, 232, 2)
moved_mask                    |  (524, 256, 232, 2)
moved_y_warped_truth          |  (524, 256, 232, 2)
fixed_y_tran                  |  (524, 2, 256, 232)
moved_y_tran                  |  (524, 2, 256, 232)
moved_y_tran_warped_truth     |  (524, 2, 256, 232)


> <img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/180142212-3dc92e7c-7e93-4be5-876f-7a86cd8e21d6.png">


### [1-2] `Planned DU DeCoLearn Dataset Structure`

`source_h5`  	     |      Shape
:---------------:   | :---------------:
x                   | (524, 2, 256, 232)
s                   | (524, 12, 256, 232)

`alignment_h5`  	|      Shape
:---------------:   | :---------------:
moved_x             | (524, 2, 256, 232)


`mri_h5`  	                    |      Shape
:---------------:             | :---------------:
fixed_y                       |  (524, 12, 256, 232, 2)
moved_y                       |  (524, 12, 256, 232, 2)
moved_y_warped_truth          |  (524, 12, 256, 232, 2)               
fixed_mask                    |  (524, 256, 232, 2)
moved_mask                    |  (524, 256, 232, 2)
fixed_y_tran                  |  (524, 2, 256, 232)
moved_y_tran                  |  (524, 2, 256, 232)
moved_y_tran_warped_truth     |  (524, 2, 256, 232)


## [2] `Performance`

### [2-1] `EDSR-based` DeCoLearn VS. DeepUnfolding-based DeCoLearn

`Zero-filled`  	                    
:---------------:             
<img width="600" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181219363-4ff31a80-464b-4c9c-a9db-60b0b7cb35d6.png">


`EDSR+Registration-based DeCoLearn`  	     |      `DeepUnfolding+Registration-based DeCoLearn`
:---------------:   | :---------------:
<img width="600" alt="IMG" src="https://user-images.githubusercontent.com/73331241/182032836-a4c50a2c-2cf9-4f5b-a9c0-1ea1eac57c83.png"> | <img width="600" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181482624-c75138dd-4d68-49c4-acd5-bec35b1af7b2.png">
<img width="600" alt="IMG" src="https://user-images.githubusercontent.com/73331241/182032878-53485b71-4bd8-4f63-9685-4b24663ee49a.png"> | <img width="600" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181490035-6de57d79-dc3e-49ee-aa56-acb1474c29e6.png">


`Groundtruth`  	                    
:---------------:             
<img width="600" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181584656-4113a19b-ddec-4b6f-9dc2-2b6540f47d81.png">




### [Study logs]: `Inverse Imaging Problem Study`
Lecture  	                    |      Lecture name            | Link
:---------------:             | :---------------:           | :---------------:
<img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181464871-a12adb4f-b331-4eb8-8d5e-ff90b5457110.png">                       |  Regularization by Artifact Removal (RARE): Imaging using Deep Priors Learned without Groundtruth     | https://www.youtube.com/watch?v=dOqNbsQbpxc
<img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181464886-a5fffb56-7af2-4556-b9d7-f3db3816755f.png">                       |  Research Seminar: "Computational Imaging" by Prof. Ulugbek Kamilov     | https://www.youtube.com/watch?v=_eAzG9mFdOw


### [Study logs]: `Encountered Problems`
Prob  	                    |      Description            | Solution
:---------------:             | :---------------           | :---------------
<img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181216426-39097dd4-19b1-4fb2-8b18-c6fefe6f82c1.png">                       |  From DU-based DeCoLearn, even though the validation error is decent, it has a dark part in model output     | `With iteration of the same EDSR, but 0.1 gamma and 1 alpha solve the problem.` <img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/183614944-4192ff95-4c34-4b81-be1c-5fbd5f0e1975.png">
<img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/181490306-76899e30-6494-4f11-8fd2-a1bdc4e463da.png">                       |  From EDSR-based DeCoLearn, training performance is also getting worse.     | `I thought the problem was overfitting. Thus, I reduced the number of resblock from 13 to 6.` <img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/182020623-e3479331-a31c-4862-a63f-64720a188413.png">
<img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/183622435-2d64824c-e3c3-44bb-9c83-4ed7932723f0.png">                       |  Even though dark parts were disappear after change gamma and alpha parameter for Deep Unfolding Network, After about 40 epochs, the performance died including training one.     | `I have changed internal blocks of DU models. Previously, I used the same EDSR blocks, but I also added CNNBlock, DnCNN.`


----


Mistake
1. Put measurement y as a CNN input (I should have put measurement y into loss function but it was my mistake.)
2. In DeCoLearn, `to_tiff` function does not have processed `post_processing` function.


Note
1. to_tiff is visualiation method
2. numpy.imag(val) return imaginary part of complex value.
3. from terminal type `tensorboard --logdir=LOG_DIR`




