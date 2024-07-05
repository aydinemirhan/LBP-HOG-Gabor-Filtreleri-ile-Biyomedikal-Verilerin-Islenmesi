# Biomedical Data Processing with Image Filters

This project explores the application of various image processing filters on biomedical data. Specifically, it utilizes ***Local Binary Patterns (LBP), Histogram of Oriented Gradients (HOG), and Gabor*** filters to process and analyze biomedical images. The aim is to enhance the quality and extract meaningful features from these images, which can be crucial for medical diagnoses and research. ***LBP*** is used for texture analysis, ***HOG*** for shape and structure detection, and ***Gabor*** filters for frequency and orientation representation. By applying these advanced filtering techniques, the project aims to improve the accuracy and reliability of biomedical data interpretation.

### Original X-ray Image

![a](https://user-images.githubusercontent.com/58745898/184991383-dcdc47e3-1e49-4415-b896-3c4094bec526.jpg)

## Local Binary Pattern <sup>4</sup>

LBP is an image processing technique that works with black and white images. It is mostly used for face detection, face recognition, object detection, and object classification. <sup>2</sup> In this technique, the image is divided into segments, and numerical operations are performed on each segment to obtain values. Each obtained value is multiplied by the corresponding value in the region weight matrix, resulting in a new set of values. This set of values is called the LBP histogram of the input image. By using histograms obtained in this way for different images, similarities between images can be detected.

### Steps

1. Divide the input image into 3x3 matrices. (Each pixel in black and white images has a value between 0-255. Therefore, the input image is divided into 3x3 segments.)
2. Calculate the value of each matrix. (How the matrix value is calculated is explained later in the text.)
3. Combine all the calculated values in order to create a new matrix.
4. Multiply each cell by the coefficients of the regions to form the final matrix. (Creating the coefficient matrix requires a separate scientific study.)

### LBP Regional Calculation Method

![lbpf](https://user-images.githubusercontent.com/58745898/184992767-f76dde8c-b3ae-453d-97b6-0f9f3924af14.png)


> gp = nth Neighboring Pixel Value

> gc = Center Pixel Value

The new values generated by the above formula represent an 8-bit number for the 3x3 matrix, obtained by consecutively placing the digits starting from the 0-0 cell in a clockwise direction. This binary number, when converted to the decimal number system, gives the value of the matrix in the decimal number system.

### What is the Global Coefficient Matrix?

The global coefficient matrix is a matrix that indicates the importance factor values of regions in an image. For instance, in an LBP-supported software used to distinguish between male and female images, the coefficient matrix should have a high coefficient for the lip region and a low coefficient for the ear region. This is because it is easier to differentiate a male from a female by the lips than by the ears. Therefore, the coefficient matrix should assign a high coefficient to the lip region, as it is a distinguishing region.<sup>1</sup>

### X-ray Image with LBP Filter

![lbp](https://user-images.githubusercontent.com/58745898/184991412-d87a7989-1509-43ce-ac16-379e98f892fe.jpg)

## Histogram of Oriented Gradients

HOG is an image processing algorithm mostly used in face and image detection, pedestrian detection, surveillance techniques in autonomous vehicles, and smart advertising.<sup>3</sup>
Its working principle involves identifying the angles contained in an object and highlighting the different ones. In short, it works similarly to edge detection algorithms.

### Working Steps<sup>3</sup>
1. **Feature Description:** A feature descriptor is a representation of an image. It simplifies the image by discarding unnecessary information and highlighting useful information.

2. **Preprocessing:** The image needs to be resized to prevent future issues. The image can be of any size, but we need to bring it to a fixed aspect ratio. The common aspect ratio is 1:2, meaning the images can be 100:200, 500:1000, etc.

3. **Gradient Calculation:** Before performing HOG, the horizontal and vertical gradients of the image need to be calculated. This is achieved by filtering the image with a kernel. Edges and important points are detected. Gaussian filtering gives the highest weight to the central pixel and decreases it for neighboring pixels depending on the window size.

4. **Calculating the Histogram of Gradients:** The image is divided into cells, and a gradient histogram is calculated for each cell. 8x8 cells contain 8x8x3 = 192 pixel values. The gradient of this patch includes 2 values (magnitude and direction) for each pixel, adding up to 8x8x2 = 128 numbers. The reason for using 8x8 cells is that they are large enough to capture features.

5. **Normalization:** Normalization can be done optionally. It is used to avoid brightness, contrast, and other lighting effects.

6. **Visualizing the Image:** Finally, this process results in an image, and the algorithm ends here.

### X-ray Image with HOG Filter

![hog](https://user-images.githubusercontent.com/58745898/184991454-c1c98345-fa68-4755-9421-405041c73640.png)

## Gabor Filter <sup>4</sup>

The Gabor Filter is an image processing filter used to detect edges extending in specific directions on an image. As a direction-based filter, the Gabor filter is frequently used in tasks such as plate recognition, character recognition, and face recognition. The process consists of the convolution of a filter matrix called the kernel matrix with the image to be filtered. The parameters of the kernel matrix are examined one by one below.

Lambda: This coefficient determines the wavelength of the cosine multiplier. If the coefficient is 1, the cosine expression will always be 1 (cos(2.pi.x') = 1), so the coefficient should be 2 or a larger integer.

Theta: Although Theta does not appear directly in the formula, it is actually one of the most important variables of the Gabor filter. This variable is used to calculate the values of x' and y' and is the orientation angle of the Gabor kernel to be created. The x' and y' variables for a given theta value are calculated using the formula below.

Phi: Phi is the phase angle of the kernel matrix to be created. By changing this value, the filter can be shifted along the x-axis.

Sigma: Sigma determines the standard deviation of the Gaussian function. As this parameter determines the openness of the Gaussian function, selecting a small value will make the Gabor wavelets closer to each other.

Gamma: This value allows the given standard deviation to be determined for y'. If this value is 1, the resulting kernel matrix will have equal standard deviations for x and y, and thus be of equal length. If a different ratio is chosen, the kernel matrix will form a rectangular shape.

x, y: These two values represent the coordinates of the 2D kernel matrix to be created. For an NxN kernel matrix, the x and y values are calculated within the range > ***[-(N-1)/2, (N-1)/2].***

The Gabor kernel calculated with the given parameters is not directly suitable for convolution. This is because the values obtained directly from the convolution operation will most likely not be between 0 and 255. Therefore, the kernel matrix is normalized so that its integral is 0. After this operation, most of the new calculated values will be between 0 and 255.

Normalization of the kernel is done with the following steps:

1. Calculate the sums of positive and negative values
2. Compute the average of the magnitudes of these two values
3. Calculate the multipliers px = Positive Sum / Average and nx = -Negative Sum / Average
4. Multiply the positive values by nx and the negative values by px

### X-ray Image with Gabor Filter

![gabor](https://user-images.githubusercontent.com/58745898/184991499-39df876e-d0cc-41cd-9347-895069972b56.jpg)

## Sources

1 : https://dergipark.org.tr/tr/download/article-file/388674

2 : https://www.youtube.com/watch?v=h-z9-bMtd7w

3 : https://aylablgn.medium.com/yönlü-gradyanlar-histogramı-histogram-of-oriented-gradients-hog-makine-öğrenmesi-5-e237f0696af3

4: http://www.cescript.com/2012/09/c-ile-gabor-filtre-uygulamasi.html
