package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.OpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.DcMotorSimple;
import com.qualcomm.robotcore.hardware.Servo;

@TeleOp (name = "TeleOp#1")
public class MeeSeeksTeleOp extends OpMode {
    private DcMotor leftmotor, rightmotor, liftmotor;
    private Servo leftservo, rightservo;


    @Override
    public void init() {
        leftmotor = hardwareMap.dcMotor.get("leftmotor");
        rightmotor = hardwareMap.dcMotor.get("rightmotor");

        leftservo = hardwareMap.servo.get("leftservo");
        rightservo = hardwareMap.servo.get("rightservo");
        liftmotor = hardwareMap.dcMotor.get("liftmotor");

        leftmotor.setDirection(DcMotorSimple.Direction.REVERSE);

    }

    @Override
    public void loop() {
        leftmotor.setPower(gamepad1.left_stick_y);
        rightmotor.setPower(gamepad1.right_stick_y);

        if (gamepad2.left_trigger > 0) {
            leftservo.setPosition(0);
            rightservo.setPosition(1);

        } else if (gamepad2.right_trigger > 0) {
            leftservo.setPosition(1);
            rightservo.setPosition(0);
        }
        liftmotor.setPower(gamepad2.right_stick_y);

    }
}
