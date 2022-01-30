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

intensity:

![Screenshot 2022-01-29 191555](https://user-images.githubusercontent.com/79502750/151687655-dd85a81f-cc3b-4670-8d15-2ab5daeb6784.png)


![Screenshot 2022-01-29 191441](https://user-images.githubusercontent.com/79502750/151687645-9ae45b8d-03d1-4ca3-bc2d-4e71b780bec6.png)

height:

![Screenshot 2022-01-29 191641](https://user-images.githubusercontent.com/79502750/151687690-67796ba6-4270-4852-aa20-578ba309b21a.png)

![Screenshot 2022-01-29 192002](https://user-images.githubusercontent.com/79502750/151687692-ea95658c-c5f2-4b44-a0f1-1c4d63ddf184.png)

Checked if visualization working:


![Screenshot 2022-01-29 222200](https://user-images.githubusercontent.com/79502750/151687826-93a690a1-aa44-4e09-9e74-a812c224717e.png)

I also used the ResNet model and implemented the `detect_objecst` function to extract 3D bounding boxes 

![Screenshot 2022-01-29 225120](https://user-images.githubusercontent.com/79502750/151687781-271f69a2-a3f9-4a77-91cf-04426b415782.png)

![Screenshot 2022-01-27 224310](https://user-images.githubusercontent.com/79502750/151687797-6260e4a0-7fb0-446e-8287-862be1d47164.png)

![Screenshot 2022-01-29 224942](https://user-images.githubusercontent.com/79502750/151687801-faf6374c-9b6a-4875-ab34-4179e77d6f40.png)

Then I used the DarkNET model, and Evaluated the model's performance


![Screenshot 2022-01-30 000345](https://user-images.githubusercontent.com/79502750/151687837-098cecc6-c7c9-4d65-9fb9-d41f690e0619.png)

