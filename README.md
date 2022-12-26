# Rover_Project
A rover perception and navigation project for detecting, locating and navigating sample rocks from a mars inspired simulated environment utilizing OpenCV and search algorithms.

# Quick Look at the Data
There's some example data provided in the test_dataset folder. This basic dataset is enough to get you up and running but if you want to hone your methods more carefully you should record some data of your own to sample various scenarios in the simulator.

Next, read in and display a random image from the test_dataset folder


path = '../test_dataset/IMG/*'
img_list = glob.glob(path)

//Grab a random image and display it

idx = np.random.randint(0, len(img_list)-1)
image = mpimg.imread(img_list[idx])
plt.imshow(image)

# Calibration Data
Read in and display example grid and rock sample calibration images. You'll use the grid for perspective transform and the rock image for creating a new color selection that identifies these samples of interest.

//In the simulator you can toggle on a grid on the ground for calibration
//You can also toggle on the rock samples with the 0 (zero) key.  
//Here's an example of the grid and one of the rocks

example_grid = '../calibration_images/example_grid1.jpg'
example_rock = '../calibration_images/example_rock1.jpg'
grid_img = mpimg.imread(example_grid)
rock_img = mpimg.imread(example_rock)
fig = plt.figure(figsize=(12,3))
plt.subplot(121)
plt.imshow(grid_img)
plt.subplot(122)
plt.imshow(rock_img)


# Perspective Transform
Define the perspective transform function from the lesson and test it on an image.

 Define a function to perform a perspective transform
 I've used the example grid image above to choose source points for the
 grid cell in front of the rover (each grid cell is 1 square meter in the sim)
 Define a function to perform a perspective transform
def perspect_transform(img, src, dst):
           
    M = cv2.getPerspectiveTransform(src, dst)
    warped = cv2.warpPerspective(img, M, (img.shape[1], img.shape[0]))# keep same size as input image
    
    return warped
    
    
Define calibration box in source (actual) and destination (desired) coordinates
These source and destination points are defined to warp the image
to a grid where each 10x10 pixel square represents 1 square meter
The destination box will be 2*dst_size on each side
dst_size = 5 
Set a bottom offset to account for the fact that the bottom of the image 
is not the position of the rover but a bit in front of it
this is just a rough guess, feel free to change it!
bottom_offset = 6

source = np.float32([[14, 140], [301 ,140],[200, 96], [118, 96]])
destination = np.float32([[image.shape[1]/2 - dst_size, image.shape[0] - bottom_offset],
                  [image.shape[1]/2 + dst_size, image.shape[0] - bottom_offset],
                  [image.shape[1]/2 + dst_size, image.shape[0] - 2*dst_size - bottom_offset], 
                  [image.shape[1]/2 - dst_size, image.shape[0] - 2*dst_size - bottom_offset],
                  ])

warped = perspect_transform(grid_img, source, destination)
plt.imshow(warped)
