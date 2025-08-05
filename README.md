# Automated Drone for Pest Detection

The automated quadcopter follows a predefined path in the field and takes pictures at various locations in the field, using an IR Camera. These images are then transfered to central unit where the pest is identified. An alert is sent to farmer saying in which location the pest is detected so he can go and take the necessary actions. 
### Pest Detection:
This is done using K Means clustering. The clustering is done based on the brightness of each pixel. The Overall Average Silhouette Score has come out to be 0.78. A labelled matrix is created where the pest is labelled as 1 and background is labelled as 0. Using regionprops function of the MATLAB image processing toolbox we can create boxes on the location where the pest is present.
The below image shows a sample output for the code.


![pest_localized_Image_6](https://github.com/user-attachments/assets/a85edada-561a-4bc1-a690-6e8619899525)



This project is partially implemented.
