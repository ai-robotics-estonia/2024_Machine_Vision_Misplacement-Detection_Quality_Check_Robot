# Testing of Automated Machine Vision-Based Program Generation Method for Collaborative Robots

## Summary
| Company Name | [Skarcon Machining OÜ](https://skarcon.ee/machinery/) |
| :--- | :--- |
| Development Team Lead Name | [Prof. Jaan Raik](https://taltech.ee/en/contacts/jaan-raik) |
| Development Team Lead E-mail | [jaan.raik@taltech.ee](mailto:jaan.raik@taltech.ee) |
| Development Team Member | Marten Roots |
| Development Team Member | Ali Emre Karatopuk |
| Development Team Member | Mohammad Hasan Ahmadilivani |
| Development Team Member | Prof. Masoud Daneshtalab |


# Description
The project validates and tests new machine vision support for collaborative robots (cobots) used in the production of manufacturing industries. The solution enables automated programming of cobots using the learned placement of the workpieces. The solution helps significantly increase the efficiency and safety of the production process and is applicable to an extensive class of production tasks.

## Objectives of the Demonstration Project
The aim of the project is to enhance the efficiency of utilizing collaborative robots (cobots) in the CNC production process. Within the project, the focus will be on employing computer vision to validate an automated method for exploiting the cobot's capabilities in assisting with production. The specific objectives include:
- Automatic calibration between the camera and the cobot,
- Detecting and measuring billet objects for programming the cobot using machine vision,
- Automated program generation and operation control for the cobot.

Also, the achievable KPIs in this project are as follows:
- Reducing the cobot setup time by 10 times (from 2 hours down to 10 minutes),
- Increasing the production rate,
- Increasing the company’s revenue by also accepting small orders.

## Activities and Results of the Demonstration Project
### Challenge

The main challenges in this project are as follows:
- **Cobot programming:** The employed robot in the workshop possesses outdated firmware which was a challenge to communicate with it. The existing solutions for remote control APIs were not well-documented, although we had a complex task. We managed to connect to the robot and communicate with it successfully after studying many documents and investigating open-source tools as well as predefined codes for the robot. To enable robot programming, we validated a comprehensive and scalable Python-based API and a set of libraries that can synthesize programs into low-level URScript instructions executable by the cobot. We connect the computer to the robot via an Ethernet cable and program the cobot fully automatically.
- **Precise and reliable object detection:** The system is expected to precisely detect, locate, and measure the metal objects to generate a safe program for the cobot. However, there are multiple challenges to fulfilling this task: 
    - **Lightning noise:** The billets are very reflective and the light in the workshop environment is non-controllable. To tackle this challenge, we configured the stereo camera configuration setup and also applied multiple image processing techniques such as image thresholding and image masking in OpenCV to reduce the effect of lightning noise. 
    - **Robustness:** The object detection algorithm must be robust to different possible errors, such as camera perspective distortion, rotation of pieces, or irrelevant objects in the camera’s frame. Various failure scenarios are considered and implemented in the system to mitigate and eliminate these errors using methods such as Hough line transform, region of interest detection, contour detection, homography calculation, and Aruco marker refinement.
    - **Measurement:** The billets should be measured with a sub-millimeter error. We achieved this precision using various AI techniques such as 6D pose estimation and homography calculation
 - **Calibration:** To generate correct objects’ locations for the cobot, the distance between the camera, to the cobot base and its gripper should be calibrated. This process was manual in the beginning, but it was a huge obstacle for our tests since manual calibration was time-consuming and highly inaccurate. Therefore, we validated an automatic eye-to-hand robot calibration process that transforms the 6D objects’ poses from the camera’s reference frame to the cobot base using linear algebra. Adjustment and validation of the automatic calibration was a complicated task due to the complexity of its mathematics and algorithms, as well as not having access to the robot during the testing in the lab. We gathered some data at the workshop and used it during the testing phase. We stimulated the robot in Blender and interfaced it with our tested solution for testing in the lab.
 - **Test and simulation:** It was not possible to go to the workshop frequently, since it was out of the city and also the cobot was not always available for testing. To enable testing and validation in the laboratory, we validated a framework in RoboDK interfaced with the camera, in which the whole system's operations could be simulated and tested. This simulation environment loads the detected pieces by the camera and visualizes the cobot operations.
 - **Safety mechanism:** There are several cases that may lead to failure during the cobot operation. Therefore, our validated system should be able to monitor the operations and ensure their safety, precision, and accuracy. We implemented a safety monitoring mechanism through a two-phase system mode: 1) initialization, and 2) execution. In the first mode, the user verifies all the operations, and in the second mode, the system monitors and guarantees the safe progress of the operations. 

### Activities implemented and results achieved
Fig. 1 illustrates the workflow of the system. The system works in two modes: initialization and execution. In the initialization mode, the user supervises the operations and confirms or corrects different expected tasks. The user enters the number of billets and their size, and then verifies if the detection and measurement of objects is valid. This is possible through the user interface. The user observes each step of the cobot's operations and confirms them. In the end, the system enters into execution mode. In the execution mode, the system performs its operations based on the saved inputs in the initialization mode. At the same time, the system monitors the operations via computer vision to ensure the safety of the operation. For example, when the cobot picks up a billet, the computer vision checks if the piece is missing.

![Fig. 1](https://github.com/user-attachments/assets/c759e3f5-99da-48d2-b09c-ab2a8964ff59)

_Fig. 1. The workflow of the implemented system._

As mentioned, calibration is a necessary step before exploiting this system. Nonetheless, the system needs to be calibrated only once, and its configuration can be reused as long as the position of the cobot or camera is not changed.
Fig. 2 shows the calibration process in the implemented system. The program generates a set of random points for the cobot in the camera’s visible frame. The camera captures the images of the Aruco code and the user should validate the images. Then, an optimization algorithm is executed to compute the corresponding transformation vectors between the camera reference point, the cobot base, and the cobot’s gripper.

![Fig. 2](https://github.com/user-attachments/assets/e1420013-183b-4288-9e53-f28b20ce1b23)

_Fig. 2. Eye-to-hand calibration._

Noteworthy that we have published a paper based on the achieved results of this project:
["Machine Vision for Automated Collaborative Robot Programming in Manufacturing Industrial Factories"](https://conference.euas.eu/2024/wp-content/uploads/2024/10/Mohammad_Hasan_Ahmadilivani_Emre_Karatopuk_Jaan_Raik.pdf), in 12th Annual Entrepreneurship and Innovation Conference (EIC), Tallinn, 2024:



### Data Sources
We captured many images in the workshop environment with the stereo camera in various configurations. Also, we took some metal pieces to the lab for testing and validation of the object detection methods. Moreover, we received some source code from the workshop that was being used by the cobot for testing purposes. 

### AI Technologies
The object detection task performed in this project mainly exploits various AI algorithms based on OpenCV. We interfaced and customized multiple algorithms for object detection and measurement to achieve high accuracy, including image thresholding, image masking, 6D pose estimation, Hough line transform, contour detection, and homography calculation. 

### Technological Results
The achieved results are as follows:
- **Measurement accuracy:** Based on our tests in both lab and workshop environments, the pieces are measured with up to 3 mm error. 
- **Localization accuracy:** The accuracy of the system depends on both calibration and object detection algorithms. Based on our experiments in the workshop, the overall error in the  best case is 0.1 mm and in the worst case is 2.5 mm across different dimensions.
- **Fast cobot setup time:** The setup time for programming the cobot, from putting the pieces on the table until the end of initialization mode with the supervision of the user, takes up to 10 minutes.



### Technical Architecture
Fig. 3 illustrates the architecture of the implemented system. The stereo camera detects the objects and monitors the operation. It is connected to an embedded computer that performs the computations. The embedded computer is connected to the cobot and generates the program for it while managing, controlling, and communicating. The embedded computer logs all operations on the PC and the user communicates with the system via software running on the PC. 

![Fig. 3](https://github.com/user-attachments/assets/043e58ea-9033-4ec3-a15a-2574aeb818b4)

_Fig. 3. The architecture of the implemented system._



### User Interface 
Fig. 4 shows how the user can see the object detection results and verify them, in the initialization mode.

![Fig. 4](https://github.com/user-attachments/assets/3ea52bbe-8df1-45f4-8476-c5be3179b2c4)

_Fig. 4. Visualization of object detection results for the user._

Furthermore, the user should supervise the cobot and its operations and correct them if necessary, and its results would be saved for the execution mode. 
All results in the execution mode are logged in a readable text file for the user, for later debugging or failure diagnosis.


### Future Potential of the Technical Solution
- **Scalability to Diverse Manufacturing Processes:** The solution can be adapted to various CNC machines and production workflows, enabling broader deployment across different industries beyond the initial use case.
- **Integration with AI for Predictive Automation:** Future enhancements can incorporate AI-driven decision-making to enable predictive task planning, anomaly detection, and adaptive control in real time.
- **Remote Monitoring and Control:** The system can be extended to support remote diagnostics, monitoring, and control, facilitating smart factory integration and reducing the need for on-site human supervision.
- **Plug-and-Play Automation Modules:** With further development, the solution can evolve into a modular, plug-and-play system, drastically lowering the technical entry barrier for SMEs adopting cobot-based automation.
- **Data-Driven Optimization for Continuous Improvement:** Accumulated production and vision data can be leveraged to continuously optimize cobot performance, calibration accuracy, and operational efficiency using data analytics and machine learning.


### Lessons Learned
- Accurate and automated calibration is required for maintaining high accuracy 
- Lab tests are not necessarily valid for the real tests at the workshop
- The robustness of the algorithm against environmental noise and vibration is vital
- Safety monitoring in such applications is crucial

