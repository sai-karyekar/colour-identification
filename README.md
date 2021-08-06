# colour-identification
Extraction of colors from images using KMeans algorithm and filtering images from a collection of images based on RGB values of colors. 
The sample_image.jpg was clicked by me and the other 5 images in the folder images were taken from Unsplash.

# Import libraries
We import the basic libraries including matplotlib.pyplot and numpy. To extract the count, we will use Counter from the collections library. To use OpenCV, we will use cv2. KMeans algorithm is part of the sklearn's cluster subpackage. To compare colors we first convert them to lab using rgb2lab and then calculate similarity using deltaE_cie76. Finally, to combine paths while reading files from a directory, we import os.

# Working with OpenCV
To read any image, we use the method cv2.imread() and specify the complete path of the image which gets imported into the notebook as a Numpy array. We can then plot it using the pyplot’s method imshow().The shape of the array is (3456, 4608, 3). The first two values match the pixels of the image. Third value is set to 3 as each pixel is represented as a combination of three colors, Red, Blue and Green.The color of the image looks a bit off. This is because, by default, OpenCV reads image in the sequence Blue Green Red (BGR). Thus, to view the actual image we need to convert the rendering to Red Green Blue (RGB).The method cvtColor allows us to convert the image rendering to a different color space. To move from BGR color space to RGB, we use the method cv2.COLOR_BGR2RGB.

# Color Identification
## RGB to Hex Conversion
We’d first define a function that will convert RGB to hex so that we can use them as labels for our pie chart.On reading the color which is in RGB space, we return a string. {:02x} simply displays the hex value for the respective color.We supply the path of the image as the argument . First, we read the file using imread and then change its color space before returning it.

## Get colors from an image
We now define the complete code as a method that we can call to extract the top colors from the image and display them as a pie chart. I’ve named the method as get_colors and it takes 3 arguments:
`image` : The image whose colors we wish to extract.
`number_of_colors`: Total colors we want to extract.
`show_chart`: A boolean that decides whether we show the pie chart or not.

First, we resize the image to the size 600 x 400. It is not required to resize it to a smaller size but we do so to lessen the pixels which’ll reduce the time needed to extract the colors from the image. KMeans expects the input to be of two dimensions, so we use Numpy’s reshape function to reshape the image data.KMeans algorithm creates clusters based on the supplied count of clusters. In our case, it will form clusters of colors and these clusters will be our top colors. We then fit and predict on the same image to extract the prediction into the variable labels.We use Counter to get count of all labels. To find the colors, we use clf.cluster_centers_. The ordered_colors iterates over the keys present in count, and then divides each value by 255. We could have directly divided each value by 255 but that would have disrupted the order.
Next, we get the hex and rgb colors. As we divided each color by 255 before, we now multiply it by 255 again while finding the colors. If show_chart is True, we plot a pie chart with each pie chart portion defined using count.values(), labels as hex_colors and colors as ordered_colors. We finally return the rgb_colors which we’ll use at a later stage.Let’s just call this method as get_colors(get_image(‘sample_image.jpg’), 8, True) and our pie chart appears with top 8 colors of the image.

# Search Images using color
We’ll now dive into the code of filtering a set of five images based on the color we’d like. For our use case, we’ll supply the RGB values for the colors Green, Blue, Black, Red and Yellow and let our system filter the images.The images are in the folder images. We define COLORS as a dictionary of colors. Then, we read all images in that folder and save their values in the images array.We first show all the images in the folder using the below mentioned for loop.We split the area into subplots equal to the number of images. The method takes the arguments as number of rows = 1, number of columns = all images i.e. 5 in our case and the index.

We first extract the image colors using our previously defined method get_colors in RGB format. We use the method rgb2lab to convert the selected color to a format we can compare. The for loop simply iterates over all the colors retrieved from the image.
For each color, the loop changes it to lab, finds the delta (basically difference) between the selected color and the color in iteration and if the delta is less than the threshold, the image is selected as matching with the color. We need to calculate the delta and compare it to the threshold because for each color there are many shades and we cannot always exactly match the selected color with the colors in the image.If we extract say 5 colors from an image, even if one color matches with the selected color, we select that image. The threshold basically defines how different can the colors of the image and selected color be.We define a function show_selected_images that iterates over all images, calls the above function to filter them based on color and displays them on the screen using imshow.We will now simply call this method and let it plot the results.

# Conclusion
In this article, we discussed the methodology to extract colors from an image using KMeans algorithm and then use this to search images based on colors.
