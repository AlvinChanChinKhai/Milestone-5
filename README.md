# Milestone-5 "Vegetable classification using STM32"

## Group Members
1. Alvin Chan Chin Khai
2. Beh Jun Long


## Table of contents
[1.0 Previous works](#Previous-works)
<br>
[2.0 Use Cases](#Use-Cases)
<br>
[3.0 System Architecture](#system-architecture)
<br>
[4.0 Online Platform and Software used](#Online-Platform-and-Software-used)
<br>
[5.0 Built and train model using Edge Impulse](#Built-and-train)
<br>
[6.0 Deploy Trained Model Into STM32](#Deploy)
<br>
[7.0 Youtube Link](#Youtube)

<a name="Previous-works"/></a>
## 1.0 Previous works
 - [High performance vegetable classification from images based on AlexNet deep learning model](https://ijabe.org/index.php/ijabe/article/view/2690/pdf)
 - [Improved Classification Approach for Fruits and Vegetables Freshness Based on Deep Learning](https://www.mdpi.com/1424-8220/22/21/8192/pdf)
 
 <a name="Use-Cases"/></a>
## 2.0 Use Cases
 - It can be used in the vegetable industries for non-destructive sorting and robotic harvesting.
 - It may also help children and visually impaired people in classifying fruits.
 - Helps to improve supermarket grocery self-checkouts.

<a name="system-architecture"/></a>
## 3.0 System Architecture
![image](https://user-images.githubusercontent.com/118173890/220200205-cc964e4b-51b1-4306-b3ae-a3d52933ead0.png)


<a name="Online-Platform-and-Software-used"/></a>
## 4.0 Online Platform and Software used

 - STM32CubeIDE \- advanced C/C++ development platform with peripheral configuration, code generation, code compilation, and debug features for STM32
 
 - Edge Impulse \- machine learning platform
 
 - STM32Cube.AI \- used to convert pre-trained neural networks into optimized code for STM32 microcontrollers


<a name="Built-and-train"/></a>
## 5.0 Built and train model using Edge Impulse

1. Download the vegetable image from the Kaggle website for training and testing purposes.
   https://www.kaggle.com/datasets/misrakahmed/vegetable-image-dataset?resource=download
   
2. After creating a new project in Edge Impulse, click on upload data aquisition -> upload data to upload the training images. Click on the choose file and select 100 image from the dataset we downloaded before. Select the automatically split into training and testing to let the system split the images into 2 category of training and testing images. Select label and enter the image class type that we uploaded such as "beans". Upload the data after make sure everything is correct. Repeat the step until all the type of vegetable that we wanted to classify is uplodaed which in this case is another 2 class of "broccoli and cabbage".
   ![image](https://user-images.githubusercontent.com/118173890/220202714-969842c8-cc43-4d5c-9ca0-40b48646ec69.png)

3. Click on the impulse design seciton to start process out input images. The image is resize to 32x32 by fitting it to shortest axists. A processing block to normalise the image and recuding color depth is added. A pre-trained model from edge impulse is also added to reduce the training time in later stage.
![image](https://user-images.githubusercontent.com/118173890/220204536-901c424f-cbe5-4625-875d-56d901f2c973.png)

4. Click on image under impulse design section. Change the color depth to greyscale and save the parameter. 
   ![image](https://user-images.githubusercontent.com/118173890/220204655-ab0fb9d1-6a30-43f0-bc83-9c59537e4520.png)
   Afterwards, it click the genetate feature to generate the feature of the 3 different types of vegetabel image we uploaded.
   ![image](https://user-images.githubusercontent.com/118173890/220205084-6b3579e3-73e5-48c2-a131-8e0e58fba992.png)

5. Go to the next section "transfer learning" under the same impulse design section, change the nural network setting to 100 training cycle, 0.0005 learn rate and 60% of variation set size. We need to make sure the trainign cycle is high enough and the learn rate is low to make sure we can get get to the bottom of the loss function and having a much higher accuracy as possible. Then click on start training to start training the model. After finish training, we can observe the accuracy and loss of our model from the training result. 
![image](https://user-images.githubusercontent.com/118992897/221177816-2a6ffc9f-305f-4949-83ca-a91b1b50581e.png)


6. After finish training we can start test our model by using the testing image that has not been used by the model using training stage. The test result will show at the model testing output.
![image](https://user-images.githubusercontent.com/118992897/221177547-597b4339-312b-40fc-97df-92828f22e18e.png)


7. Click on deployment sectiona, then click on Cube.Mx CMSIS-PACK to create the lobrary of the device that we want to deploy into. 
![image](https://user-images.githubusercontent.com/118173890/220210607-a5a6d9d1-0c93-4237-b925-8c18db40e59b.png)

  Scroll to the bottom and ckick on analyse optimization to optimize the code. After it finish blick build to built the model for deployment in the STM32 later.

  ![image](https://user-images.githubusercontent.com/118173890/220210496-87d790eb-1a54-4bbf-b151-e2137614cda0.png)




<a name="Deploy"/></a>
## 6.0 Deploy Trained Model Into STM32

1. Create a new project in STM32.

2. When creating the project, select C++ as targeted language. Then continue to built the project.

3. Open .ioc file, go to 'Pinout & Configuration' > 'Computing' > 'CRC' > click the acticated checkbox.
 ![image](https://user-images.githubusercontent.com/118992897/221048750-e861f31e-68db-40bb-b6b2-8162d9187552.png)

5. Next step is to, add the pack downloaded from Edge Impulse to the project. Select 'Help' > 'Manage Embedded Software Packages' > 'From Local...' and search for the pack file in our computer. Accept the license agreement and thepack will be installed.
 ![image](https://user-images.githubusercontent.com/118992897/221049309-4534d40a-2614-44fa-a873-0d87073d6981.png)

6. Head to the .ioc file. This time goes to 'Pinout & Configuration' > 'Software packs' > 'Select components'. Select your project, expand the pack, and tick the core. Then click 'OK' to close the window.
  ![image](https://user-images.githubusercontent.com/118992897/221050215-0d604f4d-058c-4539-82fb-73df7961ef6a.png)
  
7. Go back to 'Pinout & Configuration', under 'Software Packs', click on the project name. Tick your project under 'Mode' section. 
![image](https://user-images.githubusercontent.com/118992897/221051819-fb30d599-0a7e-4a1d-9c97-100c0e5fd8ef.png)

8. Press CTRL+S to save the workspace. Go to the project exploerer, Rename the "main.c" file under Core/Src to "main.cpp" because C++ language is used for this project.

9. Copy the C++ content and paste it in from here. (https://github.com/AlvinChanChinKhai/Milestone-5/blob/main/main.cpp)

10. Connect the St7735 TFT LCD Display to the STM32 board. 

11. Click the 'Hammer' icon to build the project. After that, connect the microcontroller and click 'Play' to start deployment our code into the STM32 board. Leave all settings as default and click'OK' to proceed. Then a message will show up to indicate our project deployment is suceed.

![image](https://user-images.githubusercontent.com/118992897/221053263-cf23e199-d5e3-4b6e-8ecd-d22776611621.png)

12. The classified vegetable should be shown on the output LCD screen. Since we are sending in the bean images, the LCD should print the "This is bean" on the output.


![image](https://user-images.githubusercontent.com/118992897/221178024-3834a739-1b33-4ebd-a36f-403e84153a1e.png)







<a name="Youtube"/></a>
## 7.0 Youtube Link
Link: https://www.youtube.com/watch?v=XrHYxpFF1N8&t=8s&ab_channel=AlvinChan
