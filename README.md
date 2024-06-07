# Image_denoising
Image denoising using three methods: Low pass Gaussian filter, Adaptive MMSE and Adaptive Shrinkage
Input image:
![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/cab11566-d075-49fe-9a3b-96036761e41f)

Grayscale+noise:

![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/860e1d0a-514c-412e-96a5-f98a4c880596)

Following are the MSE obtained in each case of filter size and sigma values:
![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/5f7617f7-33f5-4c94-b9f7-2fdda84174e2)

Hence, Filter with size 3*3 and sigma = 1 has the lowest MSE.
Denoised image with filter size 3*3 and sigma = 1:
![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/f6800f3a-1e39-4db3-b355-d804886fb195)

INFERENCE:
When denoising an image using a low pass Gaussian filter, the filter size and the standard deviation (sigma) of the Gaussian distribution are two crucial parameters that significantly affect the denoising results. Here's a qualitative comparison of the results obtained by varying these parameters:
Filter Size:
Smaller filter lengths (e.g., 3) result in less smoothing of the image but might preserve finer details and edges better.
Larger filter lengths (e.g., 11) result in more extensive smoothing, potentially reducing noise but also blurring out finer details and edges.

Standard Deviation (Sigma):
Smaller sigma values (e.g., 0.1) indicate a narrower Gaussian distribution, resulting in less smoothing and preserving finer details. However, it may not effectively remove noise if it's not well-matched to the noise characteristics.
Larger sigma values (e.g., 8) indicate a broader Gaussian distribution, resulting in more extensive smoothing and potentially over-smoothing the image, leading to loss of details.

Qualitative Comparison:
Small Filter Length & Small Sigma: The result might retain a lot of details and sharp edges but may not effectively remove noise.
Small Filter Length & Large Sigma: The result could be smoother with reduced noise, but fine details might be lost due to excessive smoothing.
Large Filter Length & Small Sigma: Might result in a balance between noise reduction and detail preservation, but some finer details might still be lost.
Large Filter Length & Large Sigma: The result might be overly smoothed, losing both noise and important details, resulting in a blurry image.
In general, there's a trade-off between noise reduction and preservation of image details. The optimal combination of filter length and sigma depends on the specific characteristics of the image and the level of noise present.

Adaptive MMSE: Compute an adaptive version of the MMSE filter where the estimates are computed for patches of size 32 × 32 with overlap of 16 in the high pass domain. All pixels belonging to multiple patches need to be assigned the average values arising from the output due to multiple patches. You need to calculate the variance of the high pass coefficients of the original image and the variance of noise in the high pass image given the noise variance σ 2 Z = 100 in the pixel domain. You cannot simulate noise in the high pass domain separately and estimate its variance.

Denoised Image (size = 3*3, sigma = 1): 
MMSE = 42.01

![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/11ef6212-3937-4ea7-ac7d-664941ddbabd)


MSE for different values of filter size and sigma:
![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/d82bca9a-bffc-4301-8943-9355efb8a4d0)

INFERENCE:
Understanding developed by solving this question:
Local Adaptability: The Adaptive MMSE filter calculates estimates for patches of the image, allowing it to adapt to local variations in noise and image structure. This adaptability can help preserve fine details while effectively reducing noise.
Patch-based Processing: By operating on patches of the image, the Adaptive MMSE filter can better preserve texture and edge information compared to global filtering approaches like the Low Pass Gaussian filter.
Complexity: Implementing the Adaptive MMSE filter involves estimating local statistics and performing filtering operations on patches, which may result in higher computational complexity compared to simpler filtering techniques.

• Adaptive Shrinkage: Shrinkage estimator on the high pass coefficients of the noisy image with the threshold optimized using SureShrink for patches of size 32 × 32. Here it is implicit that you need to the determine the threshold parameter t for every patch. (Ref: D. L. Donoho, and I. M. Johnstone, “Adapting to unknown smoothness via wavelet shrinkage,” Journal of the American Statistical Association, vol. 90, no. 432, 1995).

Adaptive SURE shrink denoised image:
Size = 3*3, sigma = 1, MMSE = 58.66

![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/c79339a1-4be8-4f40-b889-907a265b4513)

INFERENCE:
•	The qualitative aspect of adaptive shrinkage is that it balances noise reduction and detail preservation. By adapting the threshold to the image, it can shrink the coefficients more in flat regions (where the noise is more visible) and less in detailed regions (where the noise is masked by the details). This results in an image that is less noisy but still sharp and detailed.

•	However, the effectiveness of adaptive shrinkage depends on the size of the patches. If the patches are too small, the threshold may not be estimated accurately due to the lack of data. If the patches are too large, the threshold may not adapt well to the local characteristics of the image. Therefore, choosing an appropriate patch size is crucial for the performance of adaptive shrinkage.
Compare MSE of the 3 different methods (for 3*3 filter size and sigma = 1):
	     Low pass gaussian	  Adaptive MMSE	  Adaptive Shrinkage
MSE	   89.38	              42.06	          58.66

1.	Low Pass Gaussian Filter: This method is simple and computationally efficient. It works well for reducing high-frequency noise. However, it also tends to blur the image, which can result in loss of detail, especially for high-frequency components like edges. The quality of the output depends heavily on the chosen filter length and standard deviation. A larger filter length(ex-11*11) or standard deviation(ex.-8) will result in more blurring, while a smaller one might not effectively reduce the noise.
2.	Adaptive MMSE: This method is more complex and computationally intensive, but it has the potential to provide better results, especially for images with varying noise characteristics. By adapting the filter to the local characteristics of the image (computed for patches of size 32 × 32 with overlap of 16 in the high pass domain), it can preserve details better than the Gaussian filter while still reducing noise. However, the quality of the output can be affected by the accuracy of the noise and signal variance estimates. Incorrect estimates can lead to over-smoothing or under-smoothing.
3.	Adaptive Shrinkage: This method is also complex and computationally intensive, but it’s particularly effective at preserving edges and other high-frequency details. It works by shrinking the high-pass coefficients of the noisy image, with the amount of shrinkage adapted to the local(32*32) characteristics of the image. This can result in a denoised image that is less blurry and more detailed than the outputs of the other two methods. However, the effectiveness of adaptive shrinkage depends on the patch size selected and the threshold parameter.
In terms of Mean Squared Error (MSE), all three methods aim to minimize this quantity, but the Adaptive MMSE and Adaptive Shrinkage methods might achieve a lower MSE due to their adaptive nature. 
While the Adaptive MMSE and Adaptive Shrinkage methods have the potential to provide better results, the best method depends on the specific characteristics of the image and the noise, as well as the computational resources available.

Comparison plot of Low pass Gaussian and Adaptive MMSE methods:

![image](https://github.com/niveditaapr21/Image_denoising/assets/42715946/036fecf7-74dd-40c0-be2f-7cf764941d2f)


