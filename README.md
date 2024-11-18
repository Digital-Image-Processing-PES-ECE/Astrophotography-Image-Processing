# Astrophotography Image Enhancement

### Project Description:
#### Summary - Astrophotography processing using the specified techniques enhances star clarity, reduces noise, and improves image contrast effectively:

Star and Noise Sharpening (High-Pass Filtering in the Frequency Domain):
This technique enhances the visibility of stars and fine details by isolating high-frequency components, which correspond to sharp transitions in the image. It boosts the sharpness of celestial objects while suppressing smoother, low-frequency areas like the background.

Noise Removal (Median Filtering):
Median filtering is ideal for removing salt-and-pepper noise and preserving edges. By replacing each pixel with the median of its neighborhood, it reduces random noise while maintaining star boundaries and other detailed structures.

Contrast Enhancement (imadjust/Contrast Stretching):
Contrast stretching redistributes intensity values to span the full dynamic range, improving the visibility of faint details in darker regions while preserving the brightness of stars. This technique enhances image clarity and highlights important structures.

Combined Effect:
These techniques collectively enhance the aesthetic and scientific value of astrophotography by producing sharper, noise-free, and visually striking images with improved detail and contrast.

#### Course concepts used - 
1. -  Star and noise sharpening using high-pass filtering in the frequency domain 
2. -  Noise Removal by using Median Filtering technique 
3. -  Contrast Enhancement using imadjust (Contrast Stretching)
   
#### Additional concepts used -
1. -Image Deblurring using Lucy-Richardson Algorithm
   
#### Dataset - 
Link and/or Explanation if generated

#### Novelty - 
1. - Lucy-Richardson Algorithm
2. - High-pass filtering
3. - Implementing imadjust() in three different dimensions based on colour
   
### Contributors:
1. Aryan Maganally (PES1UG22EC051)
2. Bindushree R D (PES1UG22EC058)
3. K S Kaveri (PES1UG22EC110)

### Steps:
1. Clone Repository
```git clone https://github.com/Digital-Image-Processing-PES-ECE/project-name.git ```

2. Install Dependencies
```pip install -r requirements.txt```

3. Run the Code
%Image Deblurring using Lucy-Richardson Algorithm:
[filename, pathname] = uigetfile({'blurrystars.jpg', 'Desktop/blurrystars.jpg'}, 'Select a Blurred Image');
if isequal(filename, 0)
    disp('No file selected. Exiting...');
    return;
end

% Read the blurred image
blurredImage = imread(fullfile(pathname, filename));
blurredImage = im2double(blurredImage); % Normalize to [0, 1]

% Display the blurred image
figure; 
subplot(2,3,1);
imshow(blurredImage); title('Blurred Image');

%% Estimate the Point Spread Function (PSF)
% Assume a motion blur PSF for demonstration purposes
motionl = 15; % Length of motion blur in pixels
motiona = 30; % Angle of motion blur in degrees
psf = fspecial('motion', motionl, motiona);

% Display the PSF
figure; 
subplot(2,3,2);
imshow(psf, []); title('Point Spread Function (PSF)');

%% Lucy-Richardson Deconvolution
% Number of iterations
n = 30;

% Deblur the image
deblurredImage = deconvlucy(blurredImage, psf, n);

% Show the deblurred image
figure; 
subplot(2,3,3);
imshow(deblurredImage); title('Deblurred Image (Lucy-Richardson)');

%% Save the Deblurred Image
outputFile = fullfile(pathname, 'deblurred_image_lucy_richardson.png');
imwrite(deblurredImage, outputFile);
disp(['Deblurred image saved to: ', outputFile]);

%Star and noise sharpening using high-pass filtering in the frequency domain:
I = imread('stars.jpg'); 
I = im2double(I); 
R = I(:, :, 1); 
G = I(:, :, 2); 
B = I(:, :, 3);
R_star_sharpened = R;
G_star_sharpened = G;
B_star_sharpened = B;
R_noise_sharpened = R;
G_noise_sharpened = G;
B_noise_sharpened = B;
% Frequency Transform Parameters
[M, N] = size(R); 
[X, Y] = meshgrid(-N/2:N/2-1, -M/2:M/2-1);
D = sqrt(X.^2 + Y.^2);
% Filters
D0_star = 50; 
D0_noise = 20; 
H_high_star = 1 - exp(-(D.^2) / (2 * (D0_star^2)));
H_high_noise = 1 - exp(-(D.^2) / (2 * (D0_noise^2))); 
% Function to Process Each Channel
process_channel = @(channel, H_star, H_noise) deal( ...
    channel + 0.5 * real(ifft2(ifftshift(fftshift(fft2(channel)) .* H_star))), ... % Star sharpening
    channel + 1.0 * real(ifft2(ifftshift(fftshift(fft2(channel)) .* H_noise))) ... % Noise sharpening
    );
[R_star_sharpened, R_noise_sharpened] = process_channel(R, H_high_star, H_high_noise);
[G_star_sharpened, G_noise_sharpened] = process_channel(G, H_high_star, H_high_noise);
[B_star_sharpened, B_noise_sharpened] = process_channel(B, H_high_star, H_high_noise);
I_star_sharpened = cat(3, R_star_sharpened, G_star_sharpened, B_star_sharpened);
I_noise_sharpened = cat(3, R_noise_sharpened, G_noise_sharpened, B_noise_sharpened);
figure;
subplot(1, 3, 1);
imshow(I, []);
title('Original Image');
subplot(1, 3, 2);
imshow(I_star_sharpened, []);
title('Star-Sharpened Image');
subplot(1, 3, 3);
imshow(I_noise_sharpened, []);
title('Noise-Sharpened Image');

%Contrast Enhancement :
 im = imread("IMG_0749.CR2");
im_adjust = cat(3, imadjust(im(:,:,1)), imadjust(im(:,:,2)), imadjust(im(:,:,3)));
subplot(2,1,1);
imshow(im);
title('Original Image');
subplot(2,1,2);
imshow(im_adjust);
title("Enhanced Image using imadjust");

%Noise removal:
im = imread("IMG_0749.jpg");
figure;
imshow(im);
title('Original Image');

gaussian_noisy_image = imnoise(im, 'gaussian', 0, 0.01); 
figure;
imshow(gaussian_noisy_image);
title('Image with Gaussian Noise');

gaussian_noisy_image = im2double(gaussian_noisy_image);
if size(gaussian_noisy_image, 3) == 3
    
    Kmedian(:,:,1) = medfilt2(gaussian_noisy_image(:,:,1), [3 3]); 
    Kmedian(:,:,2) = medfilt2(gaussian_noisy_image(:,:,2), [3 3]); 
    Kmedian(:,:,3) = medfilt2(gaussian_noisy_image(:,:,3), [3 3]); 
else
    
    Kmedian = medfilt2(gaussian_noisy_image, [3 3]);
end

imshow(Kmedian);
title('Median filtered image');
### Outputs:
* Important intermediate steps
* Final output images 

### References:
1. - https://in.mathworks.com/discovery/image-enhancement.html
   
### Limitations and Future Work:
