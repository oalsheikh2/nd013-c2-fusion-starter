# Writeup: Track 3D-Objects Over Time

Please use this starter template to answer the following questions:

### 1. Write a short recap of the four tracking steps and what you implemented there (filter, track management, association, camera fusion). Which results did you achieve? Which part of the project was most difficult for you to complete, and why?
First, we defined the limits of the points outside our area of interest, then we use `np.where` to show points within the limits we defined by:
``` 
mask = np.where((lidar_pcl[:, 0] >= lim_x[0]) & (lidar_pcl[:, 0] <= lim_x[1]) &
                   (lidar_pcl[:, 1] >= lim_y[0]) & (lidar_pcl[:, 1] <= lim_y[1]) &
                   (lidar_pcl[:, 2] >= lim_z[0]) & (lidar_pcl[:, 2] <= lim_z[1]))
lidar_pcl = lidar_pcl[mask] 
``` 
then we implement it in `object_pcl.py` and convert the point data to an 8-bit integer in the `load_range_image` function 

```
    img_range_intensity = np.vstack((ri_range, ri_intensity))
    img_range_intensity = img_range_intensity.astype(np.uint8)
```


The output of the vizualization was the following:

![ID_1SDEX1](https://user-images.githubusercontent.com/79502750/151233548-40c55a33-5341-4af2-a244-84c5e0579614.png)

While the LiDAR point-cloud without conversion looks like this:

![ID_S2_EX1](https://user-images.githubusercontent.com/79502750/151234143-62fa88e3-4c84-48fb-ab93-3e3d64340851.png)

![ID_SD_EX2](https://user-images.githubusercontent.com/79502750/151234219-5939d302-70a9-402d-bd74-efddce58104a.png)

Then I computed the intensity and height layers of the BEV map by implementing the `bev_from_pcl` function, and the output was the following:

![ID_S2_EX11](https://user-images.githubusercontent.com/79502750/151234956-68d2f818-bd5a-45bd-b357-4fecff969d73.png)

![ID_S2_EX22](https://user-images.githubusercontent.com/79502750/151234976-60cc2886-dbb3-4d18-88b6-71e83f67f5ef.png)

I also used the ResNet model and implemented the `detect_objecst` function to extract 3D bounding boxes 

![ID_S3_EX1](https://user-images.githubusercontent.com/79502750/151235657-f7f90cc2-db6a-4bae-ab41-3d6bf732da6a.png)

![ID_S3_EX2](https://user-images.githubusercontent.com/79502750/151235752-16492206-fc28-42d8-a9db-bb54b6f2da89.png)

Then I used the DarkNET model, and Evaluated the model's performance

![ID_S4_EX3](https://user-images.githubusercontent.com/79502750/151236001-b9385bb5-8452-4164-9d08-3e337b706df4.png)


### 2. Do you see any benefits in camera-lidar fusion compared to lidar-only tracking (in theory and in your concrete results)? 
Absoulutely! LiDAR provides the physical properties of the environment around it, such as distance and speeds, while it aids in the detection of objects in a 3D point from camera 


### 3. Which challenges will a sensor fusion system face in real-life scenarios? Did you see any of these challenges in the project?
I noticed that the LiDAR sometimes casts a shadow of the objects infront of it which means missing roadway features and possibly an inability to detect objects at that blind spot, which can be depicted below:

![ID_S2_EX11](https://user-images.githubusercontent.com/79502750/151123242-b20a787d-f33d-4a98-a40b-844ec498a677.png)
### 4. Can you think of ways to improve your tracking results in the future?

We can use data from the vehicle itself such as speed.
