The system infers from an input image the number of darts (arrows) on the dashboard and the exact position of the darts (the respective zone on the board).

This project is divided into 2 tasks. Each task focuses on 1 type of dashboards:
![alt text](https://github.com/StegarescuAnaMaria/Automatic_Darts_Scoring_System_Computer_Vision/blob/main/images/1.png)

The first, "target dashboard" has 9 black and white concentric circles scored from the edge to the center with numbers from 1 to 9 and the 10th circle in the middle named bullseye.

The second, "classic dashboard" is divided into twenty numbered segments that resemble pieces of pie and are of equal size. Each pie-like segment features a double ring on the outside perimeter of the scoring area plus a triple ring on the interior area. In the center of the board stands the bullseye. Similar to the pie-like segments, the bullseye has an outer bull (the green region) and an inner bull area (red region). 

# Task 1:
The system outputs a .txt file for each image of a target dashboard containing the number of darts on the first line, and then the number of the zone (1-10) for each of the darts on the following lines.

## The model follows the next steps:
1. The cv2.inRange function is used in order to “mask” the images with darts. Thus, the binary version of the image is obtained, which contains only the barrels of each dart.
2. cv2.HoughLinesP was used in order to get a set of lines of the barrels, and then the lines are drawn on top of the original image. The previous step “faded”/partially erased the barrels, and now their full shape is rebuilt and highlighted with a brighter color than before.
3. The cv2.inRange function is applied to the new images to get the highlighted shapes, which are not "faded" like the previous.
4. The cv2.dilate will be used next for making the shapes even more noticeable.
5. cv2.findContours is used for finding the contours of each barrel. It returns arrays of coordinates of points which form the contour.
6. cv2.moments is applied to the contour to get a dictionary of values which will be used next: (M00 – the area of the barrel; M01 – the sum of each y position / column of the pixel, multiplied by the pixel intensity; M10 - the sum of each x position / row of the pixel, multiplied by the pixel intensity etc).
7. The center of each barrel is found by computing M10/M00 (x coord) and M01/M00 (y coord).
8. 

