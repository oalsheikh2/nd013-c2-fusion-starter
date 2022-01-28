# Track 3D-Objects Over Time

![Figure_4](https://user-images.githubusercontent.com/79502750/151475970-9f724935-32c7-41a1-b4a3-1dab5b926bc1.png)

![Figure_1](https://user-images.githubusercontent.com/79502750/151475982-4c51af6c-eb76-44cc-92a4-78ec15f0ab7c.png)

![Figure_3](https://user-images.githubusercontent.com/79502750/151475687-f08fa538-319e-4ddc-82d3-19ac299229a4.png)

![Figure_222](https://user-images.githubusercontent.com/79502750/151475693-7b498a1b-12c5-466d-ac78-f1807d793d44.png)

![Figure_22](https://user-images.githubusercontent.com/79502750/151475780-09c7c7d0-d454-4009-8113-30d7da772da5.png)

![Figure_2](https://user-images.githubusercontent.com/79502750/151475665-149a6522-a4c7-43b6-bbe8-faaea3bf3bd3.png)


### 1. Write a short recap of the four tracking steps and what you implemented there (filter, track management, association, camera fusion). Which results did you achieve? Which part of the project was most difficult for you to complete, and why?

The objective of the below is to calculate Mahalanobis distance between sensors using kalman filter/

For me, `trackmanagement.py` was the most difficult due to the calculation of the appropriate track score to initialize the track state

#### Step 1:

In this step I implemented functions in `filter.py` , this includes the calculation of the matrices for function `F()`, which is the the matrix for the system, and `Q()`, which is the noise covariance matrix corresponding to `F()` by the timestep `dt`

I also implemented the `predict()` function to by:

```
F = self.F()
        x1 = F*track.x
        P1 = F*track.P*F.transpose() + self.Q()
```
Then save them by: 

```
        track.set_x(x1)
        track.set_P(P1)
```
noting that `set_x`, and `set_P` are already implemented in `trackmanagement.py` 

Then I implemented the `update()` function by:
```
 H = meas.sensor.get_H(track.x) # measurement matrix
        gamma = self.gamma(track, meas)
        S = self.S(track, meas, H)
        K = track.P*H.transpose()*np.linalg.inv(S) # Kalman gain
        x1 = track.x + K*gamma # state update
        I = np.identity(self.dim_state)
        P1 = (I - K*H) * track.P # covariance update
        
```
Then saving results 

Finally, Implemented `gamma()` and `S()` functions for calculating residual and residual covariance by:

```
gamma = meas.z - meas.sensor.get_hx(track.x)

```

and

```
 return H*track.P*H.transpose() + meas.R
 
```

#### Step 2:

Implemented the function `manage_tracks()` in `trackmanagement.py` to decrease the score of unassigned tracks and delete them when P is too big or the score is too low


#### Step 3: 

Implemented `associate()` and `get_closest_track_and_meas()` functions in `association.py`

for `associate(), I placed the tracks and their measurements as matrices for `association_matrix` by implementing the `MHD()` function that calculates Mahalanobis distances by:

```
H = meas.sensor.get_H(track.x)
        gamma = KF.gamma(track, meas)
        return gamma.T * np.linalg.inv(KF.S(track, meas, H)) * gamma
```   
while for `get_closest_track_and_meas()` I calculated the minimum value of a track and measurement and have it removed from unassigned lists


#### Step 4: 
 
 Implemented the functions `in_fov()` and `get_hx()` to check of inputs from list are in the field of view of the radar and to convert the position output to camera coordinates.
 
 The function `generate_measurement()` was implemented to include frames from both camera and LiDAR by:

```
 meas = Measurement(num_frame, z, self)
        meas_list.append(meas)
        return meas_list
```

![my_tracking_results](https://user-images.githubusercontent.com/79502750/151473196-e9d83723-7648-4d2b-9af6-6c10bbad550d.gif)

### 2. Do you see any benefits in camera-lidar fusion compared to lidar-only tracking (in theory and in your concrete results)? 
 
 They both complete eachother, LiDAR produces 3D point cloud data from a laser which causes a loss of data beyond objects not in the field of view 

### 3. Which challenges will a sensor fusion system face in real-life scenarios? Did you see any of these challenges in the project?

3D objects are sometimes detected innacurately 

### 4. Can you think of ways to improve your tracking results in the future?
 fusing sensors from the floating vehicle itself such as speed for more accurate reading of distances and ultimately a better camera-lidar fusion.
