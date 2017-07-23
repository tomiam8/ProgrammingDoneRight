Code Structure
==============

Structure of Your Robot Program
-------------------------------
It would be wrong of me to say there is one right way of setting up your code. There is not. You can set up your code any way you would like, and as long as you can understand the structure and effectively navigate your code. I am going to explain one structure of code that is useful for teams using SampleRobot, non-command IterativeRobot, PeriodicRobot, and MagicRobot. It is the same structure used by 5 time Innovation in Control Award recipient Team 1418 Vae Victis. ::

    Robot/
        robot/
            autonomous/ # In this folder are separate autonomous modes
            components/ # Different components of the robot such as drive, shooter, intake, etc.
            common/ # Parts of the robot that belong to all parts, such as drivers for sensors.
            robot # Main Robot File
        tests/ # Tests for your code. Unit tests are very important.



For command based robots, the structure is pretty similar. ::

    Robot/
        robot/
            commands/ # Commands that cause the robot to perform actions
            subsystems/ # Similar to components in Iterative. Subsystems such as drive, shooter, intake, etc.
            robot # Main robot File
        tests/


RobotMap and OI
^^^^^^^^^^^^^^^
A common occurrence in robot code is a file called ``RobotMap``. This file contains **constants** use throughout the robot. Such constants include motor controller port numbers, button mapping for certain robot functions, and PID constants for your control loops. Many teams use RobotMap for keeping track of constants, but to me it makes sense for constants such as PID to be within their respective component classes, and just having the values in ``robotInit`` where you initialize all of your hardware.

``OI`` goes hand in hand with ``RobotMap``, since they both serve similar purposes. The main purpose is for all of your inputs (such as joysticks) to go into ``OI`` and the main robot program will call functions from ``OI``. A simple setup will look like this (syntax aside) ::

    teleopPeriodic() # Method inside main robot code file
        if OI.getShooter()
            shooter.spin()



    # In OI file
    getShooter()
        return Joystick.getRawButton(1)

The use of an extra file for some more readability may be a worthwhile tradeoff. Talk with your team to decide if you want to use this form of structuring your code. Both ways are fine, but the most important thing is to be consistent. It is actually *less* helpful to only have *some* of the joystick inputs in ``OI`` than to have no ``OI`` file at all.


Main Robot Structure
^^^^^^^^^^^^^^^^^^^^

You know the old saying "Cleanliness is close to godliness". The same goes for programming. Clean code -> God Tier code. The first step to having clean code is to have good organization. Let's start with ``robotInit``.

robotInit
~~~~~~~~~

.. note:: The following examples show IterativeRobot, however the same logic an be applied to Sample and Periodic.
.. tabs::

    .. code-tab:: java

        public class MyRobot(wpilib.Iterative) {

            Joystick joystick1, joystick2;
            Drive drive;

            public void robotInit(){
                joystick1 = new Joystick(0);
                joystick2 = new Joystick(1);
                drive = new Drive(new Talon(1), new Talon(2));
            }
        }

    .. code-tab:: c++

        class MyRobot : wpilib.IterativeRobot{
            Joystick joystick1, joystick2;
            Talon motor1, motor2;
            Drive drive;
        public:

            void robotInit(){
                joystick1 = Joystick(0);
                joystick2 = Joystick(1);
                drive = Drive(Talon(1), Talon(2));
            }

        }

    .. code-tab:: py

        class MyRobot(wpilib.IterativeRobot):

            def robotInit():
                self.joystick1 = Joystick(0)
                self.joystick2 = Joystick(1)
                self.motor1 = Talon(1)
                self.motor2 = Talon(2)
                self.drive = Drive(motor1, motor2)

Let's talk about what's happening in these methods. In the Java and C++ examples, the code starts with declaring the variables in the class scope (outside of any method). This allows the other methods you will use such as ``teleopPeriodic`` or ``operatorControl`` to have access to your robot components.

Inside ``robotInit`` is where we actually initializing the variables. There is no real significance to doing this inside ``robotInit`` or when you declare the variables except structure, which is what we're going for. Also notice how I never created variables for the drive motors. If you aren't going to use the variables outside of the drive class, there is no need to declare them as variables here. It makes more sense to declare them as variables inside the drive class, where you can customize them (such as setting PID if they are CANTalons, or reversing them if need be).

.. note:: If you are using RobotMap, this is where the values stored in RobotMap would be used. Instead of ``joystick1 = new Joystick(0)`` you might do ``joystick1 = new Joystick(RobotMap.LEFT_JOYSTICK)``

Teleop
~~~~~~

Now that you've created all of the robot components, we can focus on telop code. The main basis of teleop code is using ``if`` statements to check for input, and the performing some action based on these events. For example ::

    drive.drive(joystick.getY(), joystick.getX())

    if joystick.getRawButton(1)
        shootBall()

    if joystick.getRawButton(2)
        intakeBall()

    if joystick.getRawButton(3)
        climb()


This structure allows for easy configuration of joystick -> action. The drive code shouldn't involve an if statement, since you always want control over the drivetrain, and you should call the command that drives every loop. You probably want it to be at the top, that way if you have any code that edits the drive (such as angle rotation code) the values will not get overwritten by the joysticks.
