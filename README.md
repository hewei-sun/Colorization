# Automatic Line Art Colorization

## Introduction
This repository implements automatic line art colorization. In addition to training the neural network with line arts only, this repository aims to colorize the line art with several types of hints. There are mainly four types of hints.

- No hint
  - description: Colorization without hint
  - input: Line art only
  
- Atari
  - description: Colorization with hint that includes some lines in desired color (ex. PaintsChainer)
  - input: Line art and atari
  
- Tag
  - description: Colorzation with tag (ex. Tag2Pix)
  - input: Line art and tag
  
- Reference
  - description: Colorization with reference images (ex. style2paints V1)
  - input: Line art and reference image
  
## Line extraction method
There are many kinds of line extraction methods, such as XDoG or SketchKeras. However, if we train the model on only one type of line art, trained model comes to overfit and cannot colorize another type of line art adequately. Therefore, like Tag2Pix, various kinds of line art are used as the input of neural network.

I use mainly three types of line art.

- XDoG
  - Line extraction using two Gaussian distributions difference to standard deviations
  
- SketchKeras
  - Line extraction using UNet. Lines obtained by SketchKeras are like pencil drawings.
  
- Sketch Simplification
  - Line extraction using Fully-Convolutional Networks. Lines obtained by Sketch Simplification are like digital drawings.

Examples obtained by these line extraction methods are as follows.  

![](./Data/lineart.png)

Moreover, I add two types of data augmenation to line arts in order to avoid overfitting.

- Randomly morphology transformation to deal with various thicks of lines
- Randomly RGB values of lines to deal with various depths of lines

## Experiment without hint

### Motivation
First of all, I needed to confirm that methods based on neural networks can colorize without hint precisely and diversely. The training of mapping from line arts to color images is difficult because of variations in color. Therefore, I hypothesized that neural networks trained without hints would come to colorize single color in any regions. In addition to content loss, I try adversarial loss because adversarial learning enables neural networks to match data distribution adequately.

### Methods
- [x] pix2pix
- [x] pix2pixHD
- [X] bicyclegan

### Results
- pix2pix & pix2pixHD

![](./Data/nohint_comparison.png)

- bicyclegan

![](./nohint_bicyclegan/data/result1.png)

## Experiment with atari

### Motivation
Considering the application systems of colorization, we need to colorize with designated color. Therefore, I try some methods that take the hint, named atari, as input of neural network.

### Methods
- [x] userhint
- [ ] whitebox
- [ ] gaugan

### Results
- userhint
![here](./atari_userhint/data/result2.png)

- whitebox
![](./atari_whitebox/data/result2.png)

## Experiment with reference

### Motivation
I also consider taking the hint, named reference, as input of neural network. At first, I had tried to implement style2paints V1. However, I had difficulities producing the reproduction of results because training came to collapse. Then, I decide to seek for a substitute for style2paints V1.

### Methods
- [x] adain
- [x] scft
- [ ] video

### Result
- adain
![here](./reference_adain/data/res1.png)

- scft
![](./reference_scft/data/result2.png)

- video
![](./reference_video/data/never_color1.gif)
![](./reference_video/data/sakura1_color1.gif)
![](./reference_video/data/rayearth1_color1.gif)
