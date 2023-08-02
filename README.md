package org.firstinspires.ftc.teamcode.drive.opmode;

import com.qualcomm.robotcore.eventloop.opmode.Disabled;
import com.qualcomm.robotcore.eventloop.opmode.OpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.DcMotorEx;
import com.qualcomm.robotcore.hardware.DcMotorSimple;
import com.qualcomm.robotcore.hardware.Servo;


@TeleOp
public class Test extends OpMode {


    private DcMotorEx leftFront, leftRear, rightRear, rightFront, rmotor;

    int armUp = 2000;
    int armMid = 1000;
    int armDown = 0;
    private double driveSpeed = 1;
    private Servo TestServo;
    double ServoUp = 1;
    double ServoDown = 0;
    @Override
    public void init() {
        leftFront = hardwareMap.get(DcMotorEx.class, "leftFront");
        leftRear = hardwareMap.get(DcMotorEx.class, "leftRear");
        rightRear = hardwareMap.get(DcMotorEx.class, "rightRear");
        rightFront = hardwareMap.get(DcMotorEx.class, "rightFront");
        rmotor = hardwareMap.get(DcMotorEx.class, "rmotor");

        TestServo = hardwareMap.get(Servo.class, "TestServo");

        rightRear.setDirection(DcMotorSimple.Direction.REVERSE);
        rightFront.setDirection(DcMotorSimple.Direction.REVERSE);

        setDriveMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        rmotor.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        rmotor.setMode(DcMotor.RunMode.RUN_TO_POSITION);
        setDriveMode(DcMotor.RunMode.RUN_USING_ENCODER);
        rmotor.setPower(1);
        rmotor.setTargetPosition(0);
        leftFront.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        leftRear.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        rightRear.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        rightFront.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);


    }

    private void setDriveMode(DcMotor.RunMode mode)
    {
        leftRear.setMode(mode);
        leftFront.setMode(mode);
        rightRear.setMode(mode);
        rightFront.setMode(mode);

    }
    @Override
    public void loop() {


        if (gamepad2.dpad_up) {
            TestServo.setPosition(ServoUp);
        }

        else if (gamepad2.dpad_down)  {
            TestServo.setPosition((ServoDown));
        }

        drive();
    }

    public void armMovement() {
        if(gamepad2.y){
            rmotor.setTargetPosition(armDown);
        } else if (gamepad2.a) {
            rmotor.setTargetPosition(armMid);
        } else if (gamepad2.x) {
            rmotor.setTargetPosition(armDown);
        }
    }

    private void arcadeDrive(double spin, double x, double y)
    {
        leftRear.setPower((-spin +x +y)*driveSpeed);
        leftFront.setPower((-spin -x +y)*driveSpeed);
        rightRear.setPower((spin -x +y)*driveSpeed);
        rightFront.setPower((spin +x +y)*driveSpeed);

    }

    private void drive(){
        if (gamepad1.right_bumper){
            driveSpeed = 0.3;
        }
        else{
            driveSpeed = 1;
        }
        arcadeDrive(gamepad1.right_stick_x, gamepad1.left_stick_x, gamepad1.left_stick_y);


    }
}
