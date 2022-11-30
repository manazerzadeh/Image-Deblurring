# Image-Deblurring
In this project, we had a task of removing gaussian blurring and gaussian noise from images of CIFAR10 dataset. The CIFAR10 consists of 60000 32*32 color images in 10 classes, with 6000 images per class. There are 50000 training images and 10000 test images. I designed two main models, which the latter performed the best. The model, tries to remove gaussian blurring by the aid of edge detection. I will explain the details as follows.
Due to the nature of gaussian blurring, the edges are affected the most by it. This model, adapted from Edge-Aware Deep Image Deblurring [1], stresses on the fact that gaussian blurring effects edges of an image the most. So, in their proposed model, they tried to decouple the task of edge detection and the task of deblurring. By doing so, they could first estimate the edges of an image, and then pass it to the deblurring model as a fourth channel to use that information for deblurring the image. Here is a scheme of their network:
![](https://github.com/manazerzadeh/Image-Deblurring/blob/main/edge-aware-arch.PNG?raw=true)

For the EdgeNet, they had a model inspired by Holistically-nested Edge Detection [2] structure as you can see in the bottom picture.
![](https://github.com/manazerzadeh/Image-Deblurring/blob/main/Holistically_Nested.PNG?raw=true)

They also used a gan-like architecture to stabalize the training of the EdgeNet. I developed the gan edgeNet but the result was not good and the training was not stable enough. So I proceeded with the simple EdgeNet architecture. For the training part, they used canny edge detector [3] to generate the ground truth edge maps.

For the DeblurNet, they used a generative CNN architecture consisting of one global skip connection, residual convolutional blocks, and upsampling convolutional blocks. The image of the structure is found below:
![](https://github.com/manazerzadeh/Image-Deblurring/blob/main/deblurnet.PNG?raw=true)


# Conclusion & Discussion:

![image](https://user-images.githubusercontent.com/16259816/204802789-d3bbbab1-90f0-4ccd-aa31-0dbbb3000861.png)


Due to the random nature of gaussian noises it is a hard task to deblur images while preserving local features of the original image. This specificially is the case here as the size of the images are low (32 * 32). I suppose that the DeblurNet + EdgeNet can score better results on high resolution images with alot of artifacts inside the image (As it is not the case in the current Dataset). One can try to explore this model on CIFAR100 to compare the results. Also accurate hyperparameter tunning is needed which I skipped due to the timing for this project.

# References: 
[1] Fu, Zhichao, et al. "Edge-aware deep image deblurring." arXiv preprint arXiv:1907.02282 (2019).

[2] Saining Xie, Zhuowen Tu; Proceedings of the IEEE International Conference on Computer Vision (ICCV), 2015, pp. 1395-1403

[3] J. Canny, "A Computational Approach to Edge Detection," in IEEE Transactions on Pattern Analysis and Machine Intelligence, vol. PAMI-8, no. 6, pp. 679-698, Nov. 1986, doi: 10.1109/TPAMI.1986.4767851.

[4] Miyato, Takeru, et al. "Spectral normalization for generative adversarial networks." arXiv preprint arXiv:1802.05957 (2018).

[5] Buslaev, A.; Iglovikov, V.I.; Khvedchenya, E.; Parinov, A.; Druzhinin, M.; Kalinin, A.A. Albumentations: Fast and Flexible Image Augmentations. Information 2020, 11, 125. https://doi.org/10.3390/info11020125
