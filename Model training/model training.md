# Model Training

---

## Software Preparation

---

### CanMV IDE

Visit the [Kendryte Developer Community](https://www.kendryte.com/en/resource/ide,k210) and download the CanMV IDE software following the instructions in the image below, then proceed with installation.

<img src="./assets/canmv_ide.png" alt="123" style="zoom: 33%;" />

---

### MP4 Frame Extractor

Visit the [GitHub repository](https://github.com/cyc36880/mp4-frame-extractor.git) and navigate to the releases page as shown in the image below.

<img src="./assets/image-20260624085541924.png" alt="image-20260624085541924" style="zoom:33%;" />

Click the link shown to download the executable file. Please note that this software is currently available for Windows only; no installation is required—simply click to run.

<img src="./assets/image-20260624085712906.png" alt="image-20260624085712906" style="zoom:33%;" />

*\*For users in Mainland China*

You may visit the [Gitee repository](https://gitee.com/cyc36880/mp4-frame-extractor.git) and navigate to the releases page as shown below.

<img src="./assets/image-20260624090019176.png" alt="image-20260624090019176" style="zoom:33%;" />

Click the link shown to download the executable file. Please note that this software is currently available for Windows only; no installation is required—simply click to run.

<img src="./assets/image-20260624090058900.png" alt="image-20260624090058900" style="zoom:33%;" />

---

### Flashing Software

Visit the [GitHub repository](https://github.com/sipeed/kflash_gui.git) and navigate to the releases page as shown below.

<img src="./assets/image-20260624090333417.png" alt="image-20260624090333417" style="zoom:33%;" />

Click the link shown to expand all available assets.

<img src="./assets/image-20260624091059710.png" alt="image-20260624091059710" style="zoom:33%;" />

Select the version that suits your system for download. For example, Windows users can download `kflash_gui_v1.8.1_windows.7z` (a tool for extracting 7Z archives is required; please download and install one from the internet as needed).

<img src="./assets/image-20260624091302764.png" alt="image-20260624091302764" style="zoom:33%;" />

Visit the [GitHub repository](https://github.com/ICreateRobot/k210_resources/tree/main/canmv_firmware) to download the CanMV Lite firmware for this module.

| <img src="./assets/image-20260624152758662.png" alt="image-20260624152758662" style="zoom:25%;" /> | <img src="./assets/image-20260624152834645.png" alt="image-20260624152834645" style="zoom:25%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                       Click this file                        |                 Click the button to download                 |

| <img src="./assets/image-20260624093224925.png" alt="image-20260624093224925" style="zoom:33%;" /> | <img src="./assets/image-20260624093608432.png" alt="image-20260624093608432" style="zoom:25%;" /> | <img src="./assets/image-20260624093646597.png" alt="image-20260624093646597" style="zoom:25%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|    After extraction, click the file to open the software.    | If the software opens in Chinese, click the button shown to switch to English. |        Click the button to enter the erase interface.        |
| <img src="./assets/image-20260624095112730.png" alt="image-20260624095112730" style="zoom:25%;" /> | <img src="./assets/image-20260624095241464.png" alt="image-20260624095241464" style="zoom:25%;" /> | <img src="./assets/image-20260624095317976.png" alt="image-20260624095317976" style="zoom:25%;" /> |
| Select full-chip erase and choose the correct serial port according to your setup. |   Click the erase button and wait for the success pop-up.    |          Return to the firmware flashing interface.          |
| <img src="./assets/image-20260624095540367.png" alt="image-20260624095540367" style="zoom:25%;" /> | <img src="./assets/image-20260624095800480.png" alt="image-20260624095800480" style="zoom:25%;" /> | <img src="./assets/image-20260624100011988.png" alt="image-20260624100011988" style="zoom:15%;" /><img src="./assets/image-20260624095829318.png" alt="image-20260624095829318" style="zoom:15%;" /><img src="./assets/image-20260624095847675.png" alt="image-20260624095847675" style="zoom:15%;" /> |
|               Select the downloaded firmware.                | Leave all settings as shown except for the serial port, which should match your device. |       Click the flash button and wait for completion.        |

---

# Object Detection

## Preparing Image Resources

To meet the input requirements of the vision module's model (supporting 320×240 resolution images), the current workflow is as follows: First, the CanMV platform receives camera image data from the K210 development board and encodes it into an MP4 video file. Then, the MP4 frame extraction tool is used to extract the video into individual static frames. Finally, the extracted images are annotated and the model is trained on the Kendryte model training platform. This entire process is designed to achieve end-to-end data preparation and training, from image acquisition to model deployment.

---

### Recording Video with CanMV

Open CanMV IDE. Since the software defaults to Chinese, follow the instructions in the images below to change the language if needed. A restart is required for the changes to take effect.

<img src="./assets/image-20260624103216595.png" alt="image-20260624103216595" style="zoom:25%;" />

<img src="./assets/image-20260624103250369.png" alt="image-20260624103250369" style="zoom:25%;" />

Copy the following code into the IDE:

```python
import sensor, image, time, lcd

lcd.init()                   # Init lcd display
lcd.clear(lcd.RED)           # Clear lcd screen.

sensor.reset(dual_buff=True)
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)   # 320x240
sensor.set_hmirror(1)
sensor.set_vflip(1)
sensor.set_auto_whitebal(False)
sensor.skip_frames(time=2000)

while(True):
    img = sensor.snapshot()         # Take a picture and return the image.
    lcd.display(img)                # Display image on lcd.
```

| <img src="./assets/image-20260624103428724.png" alt="image-20260624103428724" style="zoom:25%;" /> | <img src="./assets/image-20260624103607140.png" alt="image-20260624103607140" style="zoom:25%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|          Click the button to connect to the device           |                 Select the appropriate port                  |
| <img src="./assets/image-20260624103748959.png" alt="image-20260624103748959" style="zoom:25%;" /> | <img src="./assets/image-20260624103911937.png" alt="image-20260624103911937" style="zoom:25%;" |
|             Wait for the connection to complete              | Paste the code above into the IDE and click the run button in the lower-left corner |
| <img src="./assets/image-20260624104134035.png" alt="image-20260624104134035" style="zoom:25%;" /> | <img src="./assets/image-20260624104300246.png" alt="image-20260624104300246" style="zoom:25%;" /> |
| The top-right corner of the software will display the image transmitted from the vision module | Click the button shown to start video recording.<br />***Please review the important notes below before recording.*** |
| <img src="./assets/image-20260624105500812.png" alt="image-20260624105500812" style="zoom:25%;" /> | <img src="./assets/image-20260624105602550.png" alt="image-20260624105602550" style="zoom:25%;" /> |
|              Click the button to stop recording              | After stopping, a save dialog will appear. Name the file as needed and save it. |
| <img src="./assets/image-20260624105828427.png" alt="image-20260624105828427" style="zoom:25%;" /> | <img src="./assets/image-20260624105948372.png" alt="image-20260624105948372" style="zoom:25%;" /> |
|                       Select as shown                        | A conversion completion pop-up will appear.<br />This completes the recording for one object. Repeat the above steps to record multiple objects. |

> Video Recording Notes: The quality of this video recording will affect the subsequent model training quality. Do not shake the camera vigorously; ensure the recorded video is clear. The target object should remain visible in the frame as much as possible. Only one target object should appear in the video; otherwise, all objects will need to be annotated later, increasing the workload. Vary the background, distance, and shooting angles—do not keep a single angle throughout, as this may cause the model to recognize objects only from a specific angle.

---

### Frame Extraction from Video

| <img src="./assets/image-20260624114515683.png" alt="image-20260624114515683" style="zoom:33%;" /> | <img src="./assets/image-20260624114537828.png" alt="image-20260624114537828" style="zoom:33%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| Open the downloaded `mp4-frame-extractor.exe` and select the recorded video |            Select the output directory for frames            |
| <img src="./assets/image-20260624114645057.png" alt="image-20260624114645057" style="zoom:33%;" /> | <img src="./assets/image-20260624114754248.png" alt="image-20260624114754248" style="zoom:33%;" |
|       Enter a prefix for the extracted frame filenames       | Keep the image format as `jpg`; the frame interval determines how many frames are skipped between extractions—generally, aim for around 100 extracted images |
| <img src="./assets/image-20260624114814136.png" alt="image-20260624114814136" style="zoom:33%;" /> |                                                              |
| Click as shown to start frame extraction. Repeat the above steps for multiple videos. |                                                              |

> Note: This software requires that the paths for the software directory, video directory, and output directory all contain only English characters; otherwise, frame extraction will fail.

---

### Annotation

Visit the [Kendryte Training Platform](https://www.kendryte.com/en/training/start) and upload the extracted frames. If this is your first time, you may be prompted to register—follow the instructions to complete registration.

| <img src="./assets/image-20260624115201095.png" alt="image-20260624115201095" style="zoom:25%;" /> | <img src="./assets/image-20260624115359224.png" alt="image-20260624115359224" style="zoom:25%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|          Navigate to the dataset interface as shown          |                    Click "Create Dataset"                    |
| <img src="./assets/image-20260624115502962.png" alt="image-20260624115502962" style="zoom:25%;" /> | <img src="./assets/image-20260624115643469.png" alt="image-20260624115643469" style="zoom:25%;" /> |
| Name your dataset as shown, select the option indicated, and confirm |         Click to enter the dataset editing interface         |
| <img src="./assets/image-20260624115748671.png" alt="image-20260624115748671" style="zoom:25%;" /> | <img src="./assets/image-20260624115853180.png" alt="image-20260624115853180" style="zoom:25%;" /> |
|         Upload all extracted frames to the platform          |                Enter the annotation interface                |
| <img src="./assets/image-20260624133135792.png" alt="image-20260624133135792" style="zoom:25%;" /> | <img src="./assets/image-20260624133502301.png" alt="image-20260624133502301" style="zoom:25%;" /> |
| Click the button shown to create labels. Here I created two labels: `control` and `microbit` | 1. Select the label to use<br />2. Select the image to annotate<br />3. Use the mouse to draw bounding boxes<br />4. Once annotated, the image will display its label<br />Proceed to annotate all images sequentially. Remember to switch labels when moving to a different object class. |
| <img src="./assets/image-20260624133947642.png" alt="image-20260624133947642" style="zoom:25%;" /> |                                                              |
| If an annotation error occurs, click on the annotated image, select delete, and then re-annotate. |                                                              |

---

## Model Training

| <img src="./assets/image-20260624134747406.png" alt="image-20260624134747406" style="zoom:25%;" /> | <img src="./assets/image-20260624134834632.png" alt="image-20260624134834632" style="zoom:25%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|  Click the button shown to return to the dataset interface   | Locate the previously annotated dataset and click the button shown to start model training |
| <img src="./assets/image-20260624135010051.png" alt="image-20260624135010051" style="zoom:25%;" /> | <img src="./assets/image-20260624135249397.png" alt="image-20260624135249397" style="zoom:25%;" /> |
| Name the model training job. Other parameters will affect the final performance. For detailed explanations, please refer to relevant documentation. If unsure, leaving them at default is acceptable. |  The model enters the queue; wait for training to complete.  |

---

# Classification

## Preparing Image Resources

To meet the input requirements of the vision module's model (supporting 128×128 resolution images), the current workflow is as follows: First, the CanMV platform receives camera image data from the K210 development board and encodes it into an MP4 video file. Then, the MP4 frame extraction tool is used to extract the video into individual static frames. Finally, the extracted images are annotated and the model is trained on the Kendryte model training platform. This entire process is designed to achieve end-to-end data preparation and training, from image acquisition to model deployment.

### Recording Video with CanMV

Copy the following code into the IDE:

```python
import sensor, image, time, lcd

lcd.init()                   # Init lcd display
lcd.clear(lcd.RED)           # Clear lcd screen.

sensor.reset(dual_buff=True)
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.B128X128)   # 128x128
sensor.set_hmirror(1)
sensor.set_vflip(1)
sensor.set_auto_whitebal(False)
sensor.skip_frames(time=2000)

while(True):
    img = sensor.snapshot()         # Take a picture and return the image.
    lcd.display(img.resize(230, 230))                # Display image on lcd.
```

Refer to the steps in the `Object Detection - Recording Video with CanMV` section.

<img src="./assets/image-20260624141459599.png" alt="image-20260624141459599" style="zoom:25%;" />

Because the resolution is 128×128, the preview image on the right will appear smaller.

> Note: Unlike object detection, classification does not require annotation; detection only indicates the probability of the target object in the current image. Therefore, when recording video, ensure the background of the target object is clean and free of clutter.

### Frame Extraction from Video

Refer to the steps in the `Object Detection - Frame Extraction from Video` section.

---

## Model Training

Visit the [Kendryte Training Platform](https://www.kendryte.com/en/training/start) and upload the extracted frames. If this is your first time, you may be prompted to register—follow the instructions to complete registration.

| <img src="./assets/image-20260624142945605.png" alt="image-20260624142945605" style="zoom:25%;" /> | <img src="./assets/image-20260624143118666.png" alt="image-20260624143118666" style="zoom:25%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|    Click the button shown to enter the dataset interface     | 1. Click "Create Dataset"<br />2. Name your dataset<br />3. Select the first model type<br />4. Click confirm to create the dataset |
| <img src="./assets/image-20260624143313455.png" alt="image-20260624143313455" style="zoom:25%;" /> | <img src="./assets/image-20260624143525224.png" alt="image-20260624143525224" style="zoom:25%;" /> |
|   Click the created dataset to enter the editing interface   | Click the button shown to upload images. Note that images must be uploaded by class—do not upload all categories at once. |
| <img src="./assets/image-20260624143835970.png" alt="image-20260624143835970" style="zoom:25%;" /> | <img src="./assets/image-20260624144046543.png" alt="image-20260624144046543" style="zoom:25%;" /> |
| In the pop-up, select all images of a single category and upload them together |                  Name the current category                   |
| <img src="./assets/image-20260624144152897.png" alt="image-20260624144152897" style="zoom:25%;" /> | <img src="./assets/image-20260624144304703.png" alt="image-20260624144304703" style="zoom:25%;" /> |
| Repeat the previous steps to upload all category images. Here I have uploaded two categories. | Click the button shown again to return to the dataset interface |
| <img src="./assets/image-20260624144533215.png" alt="image-20260624144533215" style="zoom:33%;" /> | <img src="./assets/image-20260624144656648.png" alt="image-20260624144656648" style="zoom:25%;" /> |
| Locate the prepared dataset and click the button shown to start training | 1. Name the model training job<br />2. Start model training<br />For detailed explanations of these parameters, please refer to relevant documentation. If unsure, leaving them at default is acceptable. |
| <img src="./assets/image-20260624144839981.png" alt="image-20260624144839981" style="zoom:25%;" /> |                                                              |
|             Wait for model training to complete              |                                                              |