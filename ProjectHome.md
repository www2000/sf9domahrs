**This is a Direction Cosine Matrix (DCM) based Attitude Heading Reference System (AHRS) with gyro drift correction based on accelerometer (gravity) vector and magnetometer (compass) vector.**

Hi all,

We have a full working version of the Razor 9DOF IMU AHRS :-). It´s uploaded in the Google Code repository at right.

I have included the code to read the I2C compass (HMC5843) and map the sensor axis and sensor signs (it was a little tricky...).

I have implemented the "fusion" between the 3 axis magnetometer info with our actual 6 DOF IMU code. This was my approach:

--Read the 3 axis magnetometer data

--Calculate the "tilt compensated" x and y magnetic component (standard formulation):
> CMx = mag\_x\*cos(pitch) + mag\_y\*sin(roll)**sin(pitch) + mag\_z\*cos(roll)**sin(pitch)
> CMy = mag\_y\*cos(roll) - mag\_z\*sin(roll)

--Calculate the magnetic heading with this compensated components:
> MAG\_Heading = Atan(CMy/CMx)

Now MAG\_Heading is "like" our GPS\_ground\_course, so I use the same yaw drift correction formulas that we already use with [ArduIMU+](http://code.google.com/p/ardu-imu/) but with magnetic heading instead of GPS\_ground\_course.

I have tested the code with good results. Magnetic heading is tilt compensated and there are no drift on any axis now.

I have adjusted Kp\_yaw so yaw adapts very quickly to magnetic heading (on startup it could take some seconds to adjust yaw to current magnetic heading)

I have increased the serial output baud rate to 57600 and make the output messages more compact (to increase efficiency).

Now the IMU has a "magnetic north" heading. If we know the magnetic declination of our location it´s easy to convert the magnetic north to geographic north.

For future we can add a routine to calibrate magnetometer and make some optimizations in the code.