=====
Gyros
=====

Probably the most critical sensor on any FRC robot is the gyro. The gyro (short for gyroscope) is used to measure heading, or which direction the robot is pointing. Gyros can be used to get your robot to point at a certain angle, as well as keeping an angle while driving.

Rotating to an Angle
====================

In order to rotate to an angle, we are going to use PID. There are two ways of implementing this. We can use WPILib's builtin PIDController class, or we can create our own calculator. Because rotating to an angle can be done with a simple P coefficient, we'll create our own. Here's the ::

    function calculateRotateValue(targetAngle):
        error = targetAngle - gyroAngle
        if error > threshold
            rotation =  error*kP
            return False
        else:
            rotation = 0
            return True
    
This function will take a target angle, calculate the error, and if it's within a threshold, it will return True, signaling that the robot is pointed where it needs to be. If you follow the structure of code shown in [structure] and have a class dedicated to drive, this function should go in that class. 

.. note::
    A good threshold value is 1-3 degrees, since friction will make it hard for the robot to move only one degree without overshooting.

Let's see how to use this in pseudocode ::

    function rotateToAngle(targetAngle):
        error = targetAngle - gyroAngle # check out wpilib documentation for getting the angle from the gyro
        if error > threshold
            this.rotation =  error*kP
            return False
        else:
            this.rotation = 0
            return True
    
    function move(fwd, rotation):
        // This function allows for joystick input
        this.fwd = fwd
        this.rotation = rotation
        
    function execute():
        // Execute function that should be called every loop
        this.robotdrive.drive(this.fwd, this.rotation)
        
        this.fwd = 0
        this.rotation = 0
        

In this example, you would want to call execute every iteration of teleop or autonomous. To rotate to 90 degrees::

    drive.rotateToAngle(90)
    drive.execute()

To drive straight while using joysticks::

    drive.move(joystick.Y, joystick.X) // You can still use the X joystick here
    drive.rotateToAngle(gyro.currentAngle) // It gets overwritten here
    drive.execute()

This behavior works in autonomous as well. Just give the move values (and a value of 0 for rotation) and then call your rotate function. 
