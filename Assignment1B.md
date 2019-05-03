## Assignment 1B ##
Submitted by : Abhilash Ingale (abhilashingale08@gmail.com)

### Q. What are Channels and Kernels (according to EVA)? ###
![alt_text](https://github.com/AbhilashIngale/EVA-Projects/blob/master/channel_filter.jpg)

#### Kernel / Filter/ Feature Extractor : ####
A kernel/filter/feature extractor is matrix when convolved on top of an input image, isolates one its particular characteristic. The above sketch illustrates that a filter or a kernel is similar to an actual Filter which strains out one particular components from the rest.  


#### Channel / Feature Map : ####
Suppose we have an input image (rgb) then the textures, pattern, color gradients etc. all the things that when combined together make up the input image, are called channels or feature map. In the sketch above, if an image is thoought to be as a specific combination of colors in space, then one kind of channel could be a spatial representation of one of the colors.

### Q.  Why should we only (well mostly) use 3x3 Kernels? ###
We could use 5x5 or 7x7 kernels. That would drasticaly reduce the number of layers ( by a factor of 2 and 3 respectively). However, if you look at a simple convolution operation - 
#### 199 | 7x7 | 193 & 199 | 5x5 | 195
Here, the number of parameters (weights) are 49 and 25 respectively for the required image reduction. If we use 3x3 for the same purpose - 
#### 199 | 3x3 | 197 | 3x3 | 195 | 3x3 | 193 & 199 | 3x3 | 197 | 3x3 | 195
As we see, the number of tunable weights are 27 and 18 respectively, which amounts to a massive reduction in parameters. 

#### Then, how about smaller kernels, 2x2 and 1x1 ? 
Firstly, 1x1 kernels would not learn anything with respect to their neighboring pixels as their receptive field would be only 1 pixel. So, they would be very bad at LEARNING. In case of 2x2 or any other EVEN size kernel, the output channels would undergo a possible distortion, which we might have to filter to get the actual channel required. So, 3x3 is the gloabl choice on speed optimization (not on memory though).

### Q. How many times do we need to perform 3x3 convolution operation to reach 1x1 from 199x199 (show calculations) ###
#### Approach 1 : The Math appraoch
We know that convolving by a 3x3 kernel reduces the image size by 2 pixels in either dimensions (if stride=2). So we now have a decreasing AP, where 1st term= 199 and last term= 1, common diff=2 and we have to find the number of terms (or number of convolutions requried)

##### A simple formula is : No. of terms = ((last term - 1st term)/ common diff)+1 
which gives n= (199-1)/2+1,which is 100. 

#### Approach 2 : The boring appraoch

199 | 197 | 195 | 193 | 191 | 189 | 187 | 185 | 183 | 181 | 179 | 177 | 175 | 173 | 171 | 169 | 167 | 165 | 163 | 161 | 159 | 157 | 155 | 153 | 151 | 149 | 147 | 145 | 143 | 141 | 139 | 137 | 135 | 133 | 131 | 129 | 127 | 125 | 123 | 121 | 119 | 117 | 115 | 113 | 111 | 109 | 107 | 105 | 103 | 101 | 99 | 97 | 95 | 93 | 91 | 89 | 87 | 85 | 83 | 81 | 79 | 77 | 75 | 73 | 71 | 69 | 67 | 65 | 63 | 61 | 59 | 57 | 55 | 53 | 51 | 49 | 47 | 45 | 43 | 41 | 39 | 37 | 35 | 33 | 31 | 29 | 27 | 25 | 23 | 21 | 19 | 17 | 15 | 13 | 11 | 9 | 7 | 5 | 3 | 1
##### Number of Convolutions required are : 100 
