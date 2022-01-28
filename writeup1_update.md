# Writeup: Track 3D-Objects

## When the code is functional, you are supposed to use the viewer to locate and closely inspect point-clouds on vehicles and write a short report that includes the following items:

### 1. Find and display 10 examples of vehicles with varying degrees of visibility in the point-cloud
Identify vehicle features that appear as a stable feature on most vehicles (e.g. rear-bumper, tail-lights) and describe them briefly. Also, use the range image viewer from the last example to underpin your findings using the lidar intensity channe

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
![ID_1SDEX1](https://user-images.githubusercontent.com/79502750/151233548-40c55a33-5341-4af2-a244-84c5e0579614.png)

The output of the vizualization `'show_pcl'` was the following LiDAR point-cloud:

![EX222](https://user-images.githubusercontent.com/79502750/151462311-c1844f07-babf-4f7c-94f8-7c1622701be7.png)

![EX244](https://user-images.githubusercontent.com/79502750/151462316-d5b97dca-280b-44d7-95c4-d81adf37d5c0.png)

![EX21](https://user-images.githubusercontent.com/79502750/151462318-1f301471-7d2f-4d42-8a9c-33eda05dda8b.png)

![EX24](https://user-images.githubusercontent.com/79502750/151462319-ab5ded89-1e37-459a-a654-37e3d2543703.png)

![EX26](https://user-images.githubusercontent.com/79502750/151462321-e7d36884-062d-45b7-a123-97306e21b803.png)

![ID_SD_EX2](https://user-images.githubusercontent.com/79502750/151234219-5939d302-70a9-402d-bd74-efddce58104a.png)

### 2. Identify vehicle features that appear as a stable feature on most vehicles (e.g. rear-bumper, tail-lights) and describe them briefly. Also, use the range image viewer from the last example to underpin your findings using the lidar intensity channel.

Then I computed the intensity and height layers of the BEV map by implementing the `bev_from_pcl` function, and the output was the following:


![ID_S2_EX1](https://user-images.githubusercontent.com/79502750/151234143-62fa88e3-4c84-48fb-ab93-3e3d64340851.png)

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

We can use data from the vehicle itself such as speed. And fuse more lidar sensors to avoid blind spots
