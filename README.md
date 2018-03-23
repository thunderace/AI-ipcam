# AI-ipcam
Enhancing ordinary IP cameras with AI, MQTT
(from https://harizanov.com/2018/03/enhancing-my-ordinary-security-cameras-with-ai/)

Artificial Intelligence is quickly becoming an important ingredient for IoT projects’ success, a requirement for unlocking its full potential and providing a competitive edge to those that embrace it. AI naturally integrates into existing connected sensor/actuator networks and immediately adds measurable value. It was hard to imagine until recently, that AI will be so easily accessible, with just a few libraries installed we can take benefit of this amazing technology.

Let me illustrate with a simple example – enhancing ordinary IP security cameras with AI. The goal is that the cameras will recognize the objects they see and publish the recognized object’s data to an MQTT topic. I am interested in the detected object’s type, location within the captured frame and recognition confidence level. The applications I am considering are:

Object following via PTZ (keeping the object of interest e.g. human in the middle of the frame)
PTZ camera blind spot avoidance with opposing cameras providing each other that object of interest is beyond their current viewing angle
Smart movement detection i.e. only trigger alarm event if certain object types are detected
Monitor our presence at home vs detected object of risky type, i.e. I don’t want to see humans wandering the yard while we are at work, or during nights while the security system is armed
Providing the object’s data over MQTT so other IoT nodes can make decisions/trigger actions
The project relies on image classification with deep convolutional neural networks using the darkflow library. Setting up darkflow is pretty trivial, simply follow the instructions in the repository. I used Python, basically, it starts an FFmpeg background process that captures a single frame from the RTSP stream every 5 seconds and passes it to darkflow for analysis. The analysis results are then published to an MQTT topic, and as an option, a screenshot of the detected objects is taken.

The reason why I used FFmpeg rather than OpenCV to capture the RTSP frames is that OpenCV implements RTSP over TCP by default. My specific camera model only provides RTSP over UDP. I tried to recompile with UDP support, but could not get it to work reliably. I use a RAM drive to avoid excessive writes on my SSD. Ideally, this setup will run on a Raspberry Pi 3 B+, I have one in order.
