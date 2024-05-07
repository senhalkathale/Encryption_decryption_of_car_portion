# Encryption_decryption_of_car_portion
Using matlab and affine cipher we apply encryption and decryption  on a portion of car from  an image and keeping background of that image as it is 
#code is 
PROBLEM STATEMENT :-
Write a program to :-

a. Read an RGB image containing a picture of car.

b. Use image segmentation to identity car from the image.

c. Encrypt the portion of car and keep background as it is.

d. Decrypt the portion of the encrypted car.

Code:-

% Read and display the original image
img1 = imread('C:\Users\pavan Anbhule\Downloads\car image road.jpg');
imshow(img1), title('Original Image');

% Create a region of interest (ROI) mask for the car
mask = roipoly(img1);
imshow(mask), title('binary image');

% Convert images to double for mathematical operations
image1 = double(img1);
image2 = double(mask);

% Multiply the images element-wise
car_region = image1 .* image2;
imshow(uint8(car_region)), title('Identified Car Region');

% Here we are using Affine Encryption 
% Encryption parameters
m= 255; % pixel range is 0 to 255 therefore we will take mod 255

% Prompt the user to enter values for 'a' and 'b'
a = input('Enter a value for ''a'' (1 to 255): ');
b = input('Enter a value for ''b'' (b should have a modular inverse in mod 255): ');

% Validate 'a' value: It should be in the range of 1 to 255
while a < 1 || a > 255
    disp('Invalid value for ''a''. It should be in the range of 1 to 255.');
    a = input('Re-enter a value for ''a'': ');
end

% Validate 'b' value: Its modular inverse should exist in mod 255
while gcd(b, 255) ~= 1
    disp('Invalid value for ''b''. Modular inverse does not exist in mod 255.');
    b = input('Re-enter a value for ''b'' (b should have a modular inverse in mod 255): ');
end
 
% Encrypt the car region
encrypted_car_region = mod(a * car_region + b, m);
imshow(uint8(encrypted_car_region)), title('Encrypted Car Region');

% Separate the background from the original image
background = image1 - car_region;
imshow(uint8(background)), title('Background');

% Combine the original image with the encrypted car region

sub=background + encrypted_car_region;
combined_image = image1 + encrypted_car_region;
imshow(uint8(sub)), title('Encrypted Final Image');


% Decryption parameters
a_inverse = modinv(a, m); 
b_inverse = mod(-b * a_inverse, m);

% Decrypt the encrypted car region
decrypted_car_region = mod(a_inverse * encrypted_car_region + b_inverse, m);


% Combine the background with the decrypted car region
decrypted_combined_image = background + decrypted_car_region;
imshow(uint8(decrypted_combined_image)), title('Decrypted Combined Image');

% Function to find the modular inverse

%Overall Final Output
% Display images using subplot
figure;

% Original Image
subplot(2, 2, 1);
imshow(img1), title('Original Image');

% Identified Car Region
subplot(2, 2, 2);
imshow(uint8(car_region)), title('Identified Car Region');

% Encrypted Final Image
subplot(2, 2, 3);
imshow(uint8(sub)), title('Encrypted Final Image');

% Decrypted Combined Image
subplot(2, 2, 4);
imshow(uint8(decrypted_combined_image)), title('Decrypted Combined Image');

function result = modinv(a, m)
    for i = 1:m
        if mod(a * i, m) == 1
            result = i;
            return;
        end
    end
end


# for more information contact imcasnehal@gmail.com
