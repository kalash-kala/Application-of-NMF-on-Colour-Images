# Application of Non Negative Matrix Factorisation (NMF) on Colour Images

## Objectives :

- Understand the Concept of NMF and how it can be used to extract meaningful and necessary features from the objects of consideration, in our case it is **Colour Face Images**
- Exploring ways to implement NMF for **Colour Images**. My current approach is to **run NMF for every colour channel separately** and then join their individual images to reconstruct the image
- To check which approach is better, **Two Block Coordinate Descent** or **Deep NMF using Single / Multi-Layer network**
- Approach is checked by analysing the components and final image reconstruction

## Theory / Concept :

- Let each column of the data matrix $X \in R_{+}^{p\times n}$ be a vectorised grey-level image of a face, with the $(i, j)^{th}$ entry of matrix $X$ being the intensity of the $i^{th}$ pixel in the $j^{th}$ face. NMF generates two factors $(W, H)$ so that each image $X(:, j)$ is approximated using a linear combination of the columns of $W$
- Since $W$ is nonnegative, the columns of  $W$ can be interpreted as images (that is, vectors of pixel intensities) which we refer to as the basis images. As the weights in the linear combinations are nonnegative ($H \geq 0$), these basis images can only be summed up to reconstruct each original image. Moreover, the large number of images in the data set must be reconstructed approximately with only a few basis images (in fact, $r$ is in general much smaller than $n$), hence the latter should be localised features (hence sparse) found simultaneously in several images. In the case of facial images, the basis images are features such as eyes, noses, moustaches, and lips while the columns of $H$ indicate which feature is present in which image
- A potential application of NMF is in face recognition. It has for example been observed that NMF is more robust to occlusion than PCA (which generates dense factors): in fact, if a new occluded face (e.g., with sun glasses) has to be mapped into the NMF basis, the non occluded parts (e.g., the moustache or the lips) can still be well approximated

![Screenshot 2023-09-25 at 1.32.14 PM.png](README/Application%20of%20Non%20Negative%20Matrix%20Factorisation/Screenshot_2023-09-25_at_1.32.14_PM.png)

## NMF Algorithm :

### Two-Block Coordinate Descent :

![Screenshot 2023-09-25 at 1.40.20 PM.png](README/Application%20of%20Non%20Negative%20Matrix%20Factorisation/Screenshot_2023-09-25_at_1.40.20_PM.png)

For the update function, I have used Multiplicative Update 

### Neural Networks (Deep NMF) :

- Single Layer Neural Network
- 3 Linear Layered Neural Network (Multi - Layer)

### Loss function and Optimiser :

- Loss function : Frobenius Norm → $min||X-WH||^2_F$
- Optimiser : Adam

## Approach towards the Problem :

- It is similar to the approach used for grey scale images which is
    - Create $X_{p\times n}$ where $n$ = number of images and $p$ = number of pixels in an image, so basically $(i,j)^{th}$ cell is the value of $i^{th}$ pixel in $j^{th}$ image
    - Use the above Algorithms to calculate $W,H$
- But instead of running it once, we run for each channel (RGB) separately, so we get
    - $W_{red},H_{red}$
    - $W_{green},H_{green}$
    - $W_{blue},H_{blue}$
- Then we reconstruct the image using the above values and stack them along Z dimension to create a RGB image
    - $X_{red\_reconstructed} = W_{red}\times H_{red}$
    - $X_{green\_reconstructed} = W_{green}\times H_{green}$
    - $X_{blue\_reconstructed} = W_{blue}\times H_{blue}$

## Results :

- Iterative NMF performs really well. The image reconstructions are almost similar
- Single Layer Neural Network outperforms Multi-Layer Neural Network, but is worse than Iterative method

## References :
* [The Why and How of Nonnegative Matrix Factorization, Nicolas Gillis](https://arxiv.org/pdf/1401.5226.pdf)
* [Deep NMF Topic Modeling
Jian-Yu Wang and Xiao-Lei Zhang](https://arxiv.org/pdf/2102.12998.pdf)
* [Labeled Faces in the Wild (LFW) Face Dataset](http://vis-www.cs.umass.edu/lfw/)