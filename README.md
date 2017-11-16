package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.Autonomous;
import com.qualcomm.robotcore.eventloop.opmode.Disabled;
import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.DcMotorSimple;
import com.qualcomm.robotcore.util.ElapsedTime;

@Autonomous (name = "Autonomous")
public class MeeSeeksAutonomus extends LinearOpMode {

    private static final int NEVEREST_40_REV_1_ENCODER_UNITS_PER_REVOLUTION = 1120;
    private static final double WHEEL_DIAMETER_IN_INCHES = 4;
    private static final double GEAR_RATIO = 1.5;

    private int convertInchesToEncoderUnits(double in) {
        double wheelRotations = in / (Math.PI * WHEEL_DIAMETER_IN_INCHES);
        double encoderUnitsPerWheelRotation = NEVEREST_40_REV_1_ENCODER_UNITS_PER_REVOLUTION * GEAR_RATIO;
        return (int) (wheelRotations * encoderUnitsPerWheelRotation);
    }

    @Override
    public void runOpMode() throws InterruptedException {
        AutoTransitioner.transitionOnStop(this, "TeleOp#1");
        waitForStart();
        DcMotor leftmotor = hardwareMap.dcMotor.get("leftmotor");
        DcMotor rightmotor = hardwareMap.dcMotor.get("rightmotor");


        leftmotor.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        rightmotor.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        while (leftmotor.getCurrentPosition() != 0 || rightmotor.getCurrentPosition() != 0);
        leftmotor.setMode(DcMotor.RunMode.RUN_WITHOUT_ENCODER);
        rightmotor.setMode(DcMotor.RunMode.RUN_WITHOUT_ENCODER);

        leftmotor.setPower(-.5);
        rightmotor.setPower(.5);
        leftmotor.setMode(DcMotor.RunMode.RUN_TO_POSITION);
        rightmotor.setMode(DcMotor.RunMode.RUN_TO_POSITION);
        leftmotor.setTargetPosition(leftmotor.getCurrentPosition() + convertInchesToEncoderUnits(16));
        rightmotor.setTargetPosition(rightmotor.getCurrentPosition() + convertInchesToEncoderUnits(16));

        while (opModeIsActive() && (leftmotor.isBusy() && rightmotor.isBusy()));
        leftmotor.setPower(0);
        rightmotor.setPower(0);

        while(opModeIsActive()) {
            waitTimer(1);
        }
    }

    public void waitTimer(double seconds) {
        ElapsedTime timer = new ElapsedTime();
        timer.reset();
        while(timer.seconds() < seconds) {
            idle();
