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
8. The position of the tip of the dart (x, y coordinates) is calculated with the center of the barrel in mind, assuming that the distance between the center to the tip is 131 pixels (in approximation). Consider the following figure:
![alt text](https://github.com/StegarescuAnaMaria/Automatic_Darts_Scoring_System_Computer_Vision/blob/main/images/4.png)

"D" is the center of the barrel with (x1, y1) coordinates. The red arrow is the “direction” of the tip of the dart; (x, y) are the coordinates of the tip of the dart.

x = x1 – 131 * cos (angle(A)) = x1 – 131 * AB/AC

y = y1 – 131 * sin(angle(A)) = x1 – 131 * BC/AC

Angle(A) is calculated with the help of cv2.moments: angle = 0.5 * arctan(2 * mu11/(mu20 – mu02)).
9. The position of the dart is calculated by using a dictionary of hardcoded coordinates  of contours for each dart circle. The function which_zone(point) receives coordinates and returns the correct number of the circle. The function inside_zone receives a zone number (number of circle) and coordinates of darts, then returns True if the darts falls inside the circle, and False otherwise.

#Task 2:
Task 2 follows the same image processing steps as Task 1. The only exceptions are the more intricate dictionary of hardcoded coordinates for contours of each dart board zone (the 2nd darts board is more intricate), and step 5: (cv2.findContours) returns not just the contours for the barrels, but also for some lines which I will refer to as “noise”. I added a variable called “minArea” (=3000) in order to exclude the noise, which has a smaller area compared to the barrels.
